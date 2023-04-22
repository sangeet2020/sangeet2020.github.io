---
layout: thesis
permalink: /ms-thesis/
title: Noise Robust Speech Recognition for Search and Rescue Domain
description: Masters Thesis Demo (April 2023)
date: 2023-04-05
thesis_pdf: MS_Thesis_Speech_Recognition_for_SAR_domain.pdf
ppt_pdf: S.Sagar_Masters_Defense_2023_April.pdf

authors:
  - name: Sangeet Sagar
    affiliations:
      name: Universität des Saarlandes, DFKI GmbH (Saarbrücken)
# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Emergency-vehicle-and-Siren noise
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  - name: Engine noise
  - name: Chopper noise
  - name: Static-radio noise
  - name: Breathing noise
---

<style>
         /* Add CSS styles here */
         .column {
         display: inline-block;
         width: 30%;
         padding: 10px;
         text-align: center;
         vertical-align: top;
         }
         .spectrogram {
         text-align: center;
         }
         .audio {
         width: 200px;
         height: 10px;
         background-color: #ddd;
         border-radius: 50%;
         display: inline-flex;
         justify-content: center;
         align-items: center;
         margin-bottom: 30px;
         }
         .audio:hover {
         cursor: pointer;
         background-color: #bbb;
         }
         .textbox {
            width: 100%;
            height: 70px;
            border: 1px solid #ccc;
            padding: 5px;
            margin-bottom: 15px;
            text-transform: lowercase;
            font-size: 11px; /* add the font-size property */
            line-height: 1.2;
        }

         .waveform {
         width: 100%;
         height: 100px;
         margin-bottom: 10px;
         }
         .caption {
         font-style: italic;
         font-size: 12px;
         color: #777;
         }
         .spectrogram {
         width: 100%;
         margin-top: 10px;
         }
         h4 {
        margin-bottom: 5px;
        }

         
</style>
Here we demonstrate the enhanced utterances using our SepFormer enhancement model and transcripts were generated using fine-tuned Whisper ASR. All noisy utterances are at -5 dB SNR level.
### Emergency-vehicle-and-Siren noise
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Clean Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Emergency-vehicle-and-Siren/clean.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Emergency-vehicle-and-siren/clean.svg" data-zoomable>
        <div class="caption">Waveform of clean audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Actual Transcript</b></center>
            WENN DER AUFTRAG NICHT DURCHFÜHRBAR IST ABBRECHEN WIR HABEN JETZT EIN TRUPP AUF DEM WEG ZUR VERUNFALLTEN PERSON
         </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Noisy Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Emergency-vehicle-and-Siren/noisy.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Emergency-vehicle-and-siren/noisy.svg" data-zoomable alt="Waveform of noisy audio">
        <div class="caption">Waveform of noisy audio</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Enhanced Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Emergency-vehicle-and-Siren/enhanced.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Emergency-vehicle-and-siren/enhanced.svg" data-zoomable>
        <div class="caption">Waveform of enhanced audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Predicted Transcript</b></center>
            WENN DER AUFTRAG DURCHFÜHRER IST ABRECHT WIR HABEN JETZT ÄH EIN TREPP AUF DEM WEG ZU VOR UNSERHALTEN PERSON
         </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align:center;">
            <img class="img-fluid rounded z-depth-1" 
            src="{{ site.baseurl }}/assets/img/spectrogram/Example_Emergency-vehicle-and-siren.svg" 
            data-zoomable 
            style="max-width: 70%; height: auto;">
        </div>
    </div>
</div>
<div class="caption">Log power spectrogram</div>
***

### Engine noise
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Clean Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Engine/clean.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Engine/clean.svg" data-zoomable>
        <div class="caption">Waveform of clean audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Actual Transcript</b></center>
            DIE DROHNE MELDET ZWEI PERSONEN IM OBEREN CONTAINER DES BÜROKOMPLEXES KOMMEN
         </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Noisy Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Engine/noisy.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Engine/noisy.svg" data-zoomable alt="Waveform of noisy audio">
        <div class="caption">Waveform of noisy audio</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Enhanced Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Engine/enhanced.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Engine/enhanced.svg" data-zoomable>
        <div class="caption">Waveform of enhanced audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Predicted Transcript</b></center>
            DIE DROHNE MELDET ZWEI PERSONEN IM OBEREN CONTAINER DES BÜROTKOMPLEX KOMMEN
         </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align:center;">
            <img class="img-fluid rounded z-depth-1" 
            src="{{ site.baseurl }}/assets/img/spectrogram/Example_Engine.svg" 
            data-zoomable 
            style="max-width: 70%; height: auto;">
        </div>
    </div>
</div>
<div class="caption">Log power spectrogram</div>
***

### Chopper noise
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Clean Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Chopper/clean.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Chopper/clean.svg" data-zoomable>
        <div class="caption">Waveform of clean audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Actual Transcript</b></center>
            FÜR SIE WENN SIE AUF DEN MONITOR GUCKEN LINKS ZU IHRER SEITE
         </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Noisy Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Chopper/noisy.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Chopper/noisy.svg" data-zoomable alt="Waveform of noisy audio">
        <div class="caption">Waveform of noisy audio</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Enhanced Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Chopper/enhanced.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Chopper/enhanced.svg" data-zoomable>
        <div class="caption">Waveform of enhanced audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Predicted Transcript</b></center>
            FÜR SIE WENN SIE AUF DEM MONITOR GUCKEN LINKS ZU IHRER SEITEREN
         </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align:center;">
            <img class="img-fluid rounded z-depth-1" 
            src="{{ site.baseurl }}/assets/img/spectrogram/Example_Chopper.svg" 
            data-zoomable 
            style="max-width: 70%; height: auto;">
        </div>
    </div>
</div>
<div class="caption">Log power spectrogram</div>
***

### Static-radio noise
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Clean Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Static-radio/clean.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Static-radio/clean.svg" data-zoomable>
        <div class="caption">Waveform of clean audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Actual Transcript</b></center>
            ZEHN E L W ZWO EINS VON FLORIAN NULL G W D U K EINS KOMMEN
         </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Noisy Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Static-radio/noisy.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Static-radio/noisy.svg" data-zoomable alt="Waveform of noisy audio">
        <div class="caption">Waveform of noisy audio</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Enhanced Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Static-radio/enhanced.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Static-radio/enhanced.svg" data-zoomable>
        <div class="caption">Waveform of enhanced audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Predicted Transcript</b></center>
            ZEHN E L W ZWO EINS VON FLORIAN DORTMUND NULL G W D U K EINS KOMMEN
         </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align:center;">
            <img class="img-fluid rounded z-depth-1" 
            src="{{ site.baseurl }}/assets/img/spectrogram/Example_Static-radio.svg" 
            data-zoomable 
            style="max-width: 70%; height: auto;">
        </div>
    </div>
</div>
<div class="caption">Log power spectrogram</div>
***

### Breathing noise
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Clean Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Breathing/clean.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Breathing/clean.svg" data-zoomable>
        <div class="caption">Waveform of clean audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Actual Transcript</b></center>
            FÜR EI SIE EINSATZAUFTRAG ERKUNDUNG DER HALLE INNEN MIT BODENROBOTISCHEN SYSTEMEN
         </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Noisy Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Breathing/noisy.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Breathing/noisy.svg" data-zoomable alt="Waveform of noisy audio">
        <div class="caption">Waveform of noisy audio</div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align: center;">
            <h4 style="text-transform: none;">Enhanced Audio</h4>
        </div>
        <div class="audio">
            <audio controls>
               <source src="{{ site.baseurl }}/assets/img/audio_files/Breathing/enhanced.wav" type="audio/wav">
            </audio>
         </div>
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/waveform/Breathing/enhanced.svg" data-zoomable>
        <div class="caption">Waveform of enhanced audio</div>
        <div class="textbox">
            <center><b style="text-transform: none;">Predicted Transcript</b></center>
                JA FÜR SIE EINSATZAUFTRAG ERKUNDUNG DER HALLE IN MIT BODENROBOTISCHEN SYSTEMEN
         </div>
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <div style="text-align:center;">
            <img class="img-fluid rounded z-depth-1" 
            src="{{ site.baseurl }}/assets/img/spectrogram/Example_Breathing.svg" 
            data-zoomable 
            style="max-width: 70%; height: auto;">
        </div>
    </div>
</div>
<div class="caption">Log power spectrogram</div>
***

<div class="acknowledgement" style="font-size: 14px; font-style: italic;">
    <h2>Acknowledgement</h2>
    <p>Special thanks to my supervisors and advisors for their guidance and support throughout the project. This work was supported under the project A-DRZ: Setting up the German Rescue Robotics Center and funded by the German Ministry of Education and Research (BMBF), grant No. I3N14856.</p>
</div>