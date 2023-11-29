---
layout: post
title: icefall- Data Prepration Pipeline
date:  2023-08-26 16:40:16
description: This tutorial explains how data prepration is performed in icefall recipes. 
# tags: formatting links
categories: tutorial
---

## Data Prepration

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="max-width: 900px;">
        {% include figure.html path="assets/img/Icefall- LibriSpeech.drawio.svg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

icefall relies on Lhotse library for data prepration. It makes audio data preparation for all recipes. Lets study and investigate whats happening at each stage.

1. **First, manifests of the data are prepared**<br>
    ```bash
    lhotse prepare commonvoice -j 1 $commonvoice_dir data/manifests
    ```
    In `data/manifests` you can find files like
    ```bash
    data/manifests/commonvoice_recordings_train.jsonl.gz 
    data/manifests/commonvoice_supervisions_train.jsonl.gz
    ```

    Let's take a look at the contents of these files:

    ```bash
    gzip -cd data/manifests/commonvoice_recordings_train.jsonl.gz | head -n 1
    ```

    Sample content:

    ```json
    {
    "id":"sample_09901.wav",
    "sources":[
        {
            "type":"file",
            "channels":[
                0
            ],
            "source":"/home/audio/sample_09901.wav"
        }
    ],
    "sampling_rate":16000,
    "num_samples":1769149,
    "duration":110.5718125,
    "channel_ids":[
        0
    ]
    }
    ```

    ```bash
    gzip -cd data/manifests/commonvoice_supervisions_train.jsonl.gz | head -n 1
    ```

    Sample content:

    ```json
    {
    "id":"sample_09901.wav",
    "recording_id":"sample_09901.wav",
    "start":0.0,
    "duration":110.5704,
    "channel":0,
    "text":"~MUSIC i like spongebob.. so does my neighbour",
    "language":"de",
    "speaker":"0000000013-spk1_deu",
    "custom":{
        "utt_id":"rec-037864_deu",
        "end":110.5704
    }
    }
    ```
<br>

2. **Pre-processing of Manifests**<br>
    The next step is the pre-processing of these manifests using:

    ```bash
    ./local/preprocess_commonvoice.py
    ```

    This involves cleaning and normalizing transcripts for train/test/valid data. It inputs
    ```bash
    data/manifests/commonvoice_recordings_train.jsonl.gz
    data/manifests/commonvoice_supervisions_train.jsonl.gz
    ```

    The processed cuts are stored in `data/fbank/commonvoice_cuts_train_raw.jsonl.gz`.
<br>

3. **Further, this cut is split into 1000 pieces (`num_splits`)**<br>
    ```bash
    split_dir=data/fbank/commonvoice_train_split_100/
    lhotse split 1000 -i 1 ./data/fbank/commonvoice_cuts_train_raw.jsonl.gz $split_dir
    ```

    However, this split is only done for the train dataset and not for other sets. These cuts are stored in:

    - `data/fbank/commonvoice_train_split_1000/commonvoice_cuts_train_raw.1.jsonl.gz`
    - `data/fbank/commonvoice_train_split_1000/commonvoice_cuts_train_raw.2.jsonl.gz`
    - `data/fbank/commonvoice_train_split_1000/commonvoice_cuts_train_raw.3.jsonl.gz`
    - ... and so on.
<br>

4. **Each of these cuts is then processed, and fbank features are computed**<br>
    ```bash
    ./local/compute_fbank_commonvoice_splits.py --num-workers $nj --batch-duration 200 --start 0 --num-splits $num_splits
    ```

    Here is how the processing takes place:

    ```bash
    Processing 1/1000
    Loading data/fbank/commonvoice_train_split_1000/commonvoice_cuts_train_raw.1.jsonl.gz
    Filtering out super short utts
    Removed 0 cuts from 1000 cuts. 0.000% data is removed.
    Doing speed perturb (train set only)
    Splitting cuts into smaller chunks.
    Computing features
    Computing features in batches
    Saving to data/fbank/commonvoice_train_split_1000/commonvoice_cuts_train.1.jsonl.
    ```

    At the end of each processing step, two files are created in `data/fbank/commonvoice_train_split_1000/`:

    - `commonvoice_feats_train_1.lca`: fbank features stored in compressed format (lilcom_chunky: *.lca)
    - `commonvoice_cuts_train.1.jsonl.gz`: new cut stored after filtering short sentences, doing speed perturbation, etc. For the next steps, this cut shall be used and not the *raw* one.
<br>

5. **Feature Combination**<br>
    Next, the features are combined for the train set using:

    ```bash
    lhotse combine $pieces data/fbank/commonvoice_cuts_train.jsonl.gz
    ```

    This takes all the new cuts and combines them into one file `data/fbank/commonvoice_cuts_train.jsonl.gz`. Further, it shuffles them.
<br>

6. **Transcript Creation and BPE Models** <br>
    Finally, the train transcripts are created from `data/fbank/commonvoice_cuts_train.jsonl.gz`, and BPE models are created.
