---
layout: post
title: icefall- Feature Extraction Pipeline
date:  2023-08-26 16:40:16
description: This tutorial walks you through how feature extraction is done in icefall.
# tags: formatting links
categories: tutorial
---

## Feature extraction
Lets see what happens in `local/compute_fbank_commonvoice_splits.py`

```bash
extractor = KaldifeatFbank(KaldifeatFbankConfig(device=device))
```


```json
{
    'config': KaldifeatFbankConfig(
                frame_opts=KaldifeatFrameOptions(
                    sampling_rate=16000, 
                    frame_shift=0.01, 
                    frame_length=0.025, 
                    dither=0.0, 
                    preemph_coeff=0.97, 
                    remove_dc_offset=True, 
                    window_type='povey', 
                    round_to_power_of_two=True, 
                    blackman_coeff=0.42, 
                    snip_edges=False
                ), 
            mel_opts=KaldifeatMelOptions(
                    num_bins=80, 
                    low_freq=20.0, 
                    high_freq=-400.0, 
                    vtln_low=100.0, 
                    vtln_high=-500.0, 
                    debug_mel=False, 
                    htk_mode=False
                ), 
            use_energy=False, 
            energy_floor=1e-10, 
            raw_energy=True, 
            htk_compat=False, 
            use_log_fbank=True, 
            use_power=True, 
            device=device(type='cpu'), 
            chunk_size=120000
        ), 
        'extractor': Fbank()
}
```
This is used to compute features..
```python
logging.info("Computing features")
cut_set = cut_set.compute_and_store_features_batch(
           extractor=extractor,
           storage_path=f"{output_dir}/commonvoice_feats_{subset}_{idx}",
           num_workers=args.num_workers,
           batch_duration=args.batch_duration,
           storage_type=LilcomChunkyWriter,
           overwrite=True,
       )
```
The function `compute_and_store_features_batch` in code above is found in `lhotse/lhotse/cut/set.py(2102)`
<br>

Lets see what this `feat_manifest` is
```python
Features(
    type='kaldifeat-fbank', 
    num_frames=11057, 
    num_features=80, 
    frame_shift=0.01, 
    sampling_rate=16000, 
    start=0.0, 
    duration=110.570375, 
    storage_type='lilcom_chunky', 	
    storage_path='data/fbank/commonvoice_train_split_1000/commonvoice_feats_train_0001.lca', 	
    storage_key='0,40733,41792,41952,41943,42282,41001,40090,39964,40992,41959,41793,41068,41138,41037,41167,41398,41026,41822,41264,40818,41864,37953,5273', 
    recording_id='None', 
    channels=0
)
```

## Cuts
Contents of
```bash
gzip -cd data/fbank/commonvoice_cuts_valid_raw.jsonl.gz | head -n 1
```
This is basically how cut looks like
```json
{
   "id":"sample_001-0",
   "start":0,
   "duration":110.1538125,
   "channel":0,
   "supervisions":[
      {
         "id":"sample_001",
         "recording_id":"sample_001",
         "start":0.0,
         "duration":110.1525,
         "channel":0,
         "text":"<unk> SOME TRANSCRIPT GOES HERE ...",
         "language":"de",
         "speaker":"0000000025-spk1_deu",
         "custom":{
            "utt_id":"recording_001_speaker_002",
            "end":110.1525
         }
      }
   ],
   "recording":{
      "id":"sample_001",
      "sources":[
         {
            "type":"file",
            "channels":[
               0
            ],
            "source":"sample_001.wav"
         }
      ],
      "sampling_rate":16000,
      "num_samples":1762461,
      "duration":110.1538125,
      "channel_ids":[
         0
      ]
   },
   "type":"MonoCut"
}
```

