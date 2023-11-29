---
layout: post
title: icefall- Audio read
date:  2023-08-26 16:40:16
description: This tutorial describes where does audio reading and feature extraction take place in icefall.
# tags: formatting links
categories: tutorial
---

## Where exactly audio read and feature extraction (KaldiFeat fbank) is done in Lhotse?

### Audio read
The process begins with the creation of a UnsupervisedWaveformDataset object in Lhotse.
```bash
dataset = UnsupervisedWaveformDataset(collate=collate)
```

A DataLoader is then created from the dataset [see: `lhotse/cut/set.py`].
```bash
dloader = DataLoader( dataset, batch_size=None, sampler=sampler, num_workers=num_workers )
```

Inside `UnsupervisedWaveformDataset` class (`lhotse/lhotse/dataset/unsupervised.py`), `__getitem__` returns 
```python
c.load_audio()
{
"cuts": cuts,
"audio": audio,
}
```

So when we iterate through the `dloader`, we get `cuts` and `audio` tensor -
```python
for batch in dloader:
	cuts = batch["cuts"]
	waves = batch["audio"]
```

To get `audio` tensor, we can simply do
```python
for cut in cut_set: 
   print(cut.load_audio())
```

Further to extract features in `set.py`, 
```python
features = extractor.extract_batch( waves, sampling_rate=cuts[0].sampling_rate, lengths=wave_lens )
```

Extractor has been explained [here](https://sangeet2020.github.io/blog/2023/feature_extraction/).  The function `extract_batch` is present in `lhotse/features/kaldifeat.py`. The actual Kaldifeat fbank feature extraction takes place in here (`lhotse/features/kaldifeat.py`) – this is because the extractor specifies the type of feature I.e. `Kaldifeat` fbank) - 

```python
import kaldifeat 
self.extractor = kaldifeat.Fbank(kaldifeat.FbankOptions.from_dict(self.config.to_dict()) )
# Actual feature extraction. 
result = self.extractor(samples, chunk_size=self.config.chunk_size)
```

Once the Kaldifeat fbank features are computed, they are sent to further processing - 
```python
_save_worker(cuts, features)- this is done in lhotse/cut/set.py			
```

No matter what start or end times are specified, the feature extraction process will compute features for the entire audio file. If the associated transcript for the audio file specifies a segment, such as from 5 seconds to 8 seconds within a 20-minute long audio, the computed features should be trimmed to this specific segment before being saved to the LCA-compressed file. Lets see how this is done in 
_save_worker(cuts, features)

Inside the below function, the actual trimming is done (`set.py`)
```python
# Features= lhotse.features.base.Features
feat_manifest = Features( 
      start=cut.start, 
      duration=cut.duration, 
      type=extractor.name, 
      num_frames=feat_mat.shape[0], 
      num_features=feat_mat.shape[1], 
      frame_shift=frame_shift, 
      sampling_rate=cut.sampling_rate, 
      channels=cut.channel, 
      storage_type=feats_writer.name, 
      storage_path=str(feats_writer.storage_path), 
      storage_key=storage_key
)
```

This actually writes the feature on the disk.
```python
storage_key = feats_writer.write(cut.id, feat_mat) 
```

Returns keys like: "14601,31,23,42". The first number is the offset for the whole array,  and the following numbers are relative offsets for each chunk. These offsets are relative to the previous chunk start.
