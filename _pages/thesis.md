---
layout: page
permalink: /thesis/
title: 
description: 
nav: false
nav_order: -1
---


<html>
   <head>
      <title>Noise Robust Speech Recognition Demo</title>
      <style>
         /* Add CSS styles here */
         h1 {
         text-align: center;
         }
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
         width: 300px;
         height: 25px;
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
         height: 60px;
         border: 1px solid #ccc;
         padding: 5px;
         margin-bottom: 10px;
         text-transform: lowercase;
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
      </style>
   </head>
   <body>
      <h1 style="font-size: 36px;">Noise Robust Speech Recognition for Search and Rescue Domain</h1>
      <h1 style="font-size: 28px;">Masters Thesis Demo</h1>
      <div style="text-align: center;">
         <p style="font-size: 28px;">by</p>
      </div>
      <h1 style="font-size: 28px;">Sangeet Sagar</h1>
      <div style="text-align: center;">
         <p style="font-size: 28px;">April 2023</p>
      </div>
      <h2>1. Emergency-vehicle-and-Siren noise (-5 dB)</h2>
      <div class="column">
         <h3>Clean Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Emergency-vehicle-and-Siren/clean.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Actual Transcript</b><br>
            WENN DER AUFTRAG NICHT DURCHFÜHRBAR IST ABBRECHEN WIR HABEN JETZT EIN TRUPP AUF DEM WEG ZUR VERUNFALLTEN PERSON
         </div>
         <img class="spectrogram" src="waveform/Emergency-vehicle-and-siren/clean.svg" alt="Waveform of clean audio">
         <div class="caption">Waveform of clean audio</div>
      </div>
      <div class="column">
         <h3>Noisy Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Emergency-vehicle-and-Siren/noisy.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            WENN DER AUFTRAG DURCHFÜHRER IST ABRECHT WIR HABEN JETZT ÄH EIN TREPP AUF DEM WEG ZU VOR UNSERHALTEN PERSON
         </div>
         <img class="spectrogram" src="waveform/Emergency-vehicle-and-siren/noisy.svg" alt="Waveform of noisy audio">
         <div class="caption">Waveform of noisy audio</div>
      </div>
      <div class="column">
         <h3>Enhanced Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Emergency-vehicle-and-Siren/enhanced.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            WENN DER AUFTRAG DURCHFÜHRER IST ABRECHT WIR HABEN JETZT ÄH EIN TREPP AUF DEM WEG ZU VOR UNSERHALTEN PERSON
         </div>
         <img class="spectrogram" src="waveform/Emergency-vehicle-and-siren/enhanced.svg" alt="Waveform of enhanced audio">
         <div class="caption">Waveform of enhanced audio</div>
      </div>
      <div class="spectrogram">
         <h4>Log Power Spectrogram</h4>
         <img src="spectrogram/Example_Emergency-vehicle-and-siren.svg" alt="Log power spectrogram of noisy audio">
         <div class="caption">Log power spectrogram of noisy audio</div>
      </div>
      <!-- ********************************************* -->
      <h2>2. Engine noise (-5 dB)</h2>
      <div class="column">
         <h3>Clean Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Engine/clean.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Actual Transcript</b><br>
            DIE DROHNE MELDET ZWEI PERSONEN IM OBEREN CONTAINER DES BÜROKOMPLEXES KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Engine/clean.svg" alt="Waveform of clean audio">
         <div class="caption">Waveform of clean audio</div>
      </div>
      <div class="column">
         <h3>Noisy Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Engine/noisy.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            DIE DROHNE MELDET ZWEI PERSONEN IM OBEREN CONTAINER DES BÜROTKOMPLEX KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Engine/noisy.svg" alt="Waveform of noisy audio">
         <div class="caption">Waveform of noisy audio</div>
      </div>
      <div class="column">
         <h3>Enhanced Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Engine/enhanced.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            DIE DROHNE MELDET ZWEI PERSONEN IM OBEREN CONTAINER DES BÜROTKOMPLEX KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Engine/enhanced.svg" alt="Waveform of enhanced audio">
         <div class="caption">Waveform of enhanced audio</div>
      </div>
      <div class="spectrogram">
         <h4>Log Power Spectrogram</h4>
         <img src="spectrogram/Example_Engine.svg" alt="Log power spectrogram of noisy audio">
         <div class="caption">Log power spectrogram of noisy audio</div>
      </div>
      <!-- ********************************************* -->
      <h2>3. Chopper noise (-5 dB)</h2>
      <div class="column">
         <h3>Clean Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Chopper/clean.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Actual Transcript</b><br>
            FÜR SIE WENN SIE AUF DEN MONITOR GUCKEN LINKS ZU IHRER SEITE 
         </div>
         <img class="spectrogram" src="waveform/Chopper/clean.svg" alt="Waveform of clean audio">
         <div class="caption">Waveform of clean audio</div>
      </div>
      <div class="column">
         <h3>Noisy Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Chopper/noisy.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            FÜR SIE WENN SIE AUF DEM MONITOR GUCKEN LINKS ZU IHRER SEITEREN
         </div>
         <img class="spectrogram" src="waveform/Chopper/noisy.svg" alt="Waveform of noisy audio">
         <div class="caption">Waveform of noisy audio</div>
      </div>
      <div class="column">
         <h3>Enhanced Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Chopper/enhanced.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            FÜR SIE WENN SIE AUF DEM MONITOR GUCKEN LINKS ZU IHRER SEITEREN
         </div>
         <img class="spectrogram" src="waveform/Chopper/enhanced.svg" alt="Waveform of enhanced audio">
         <div class="caption">Waveform of enhanced audio</div>
      </div>
      <div class="spectrogram">
         <h4>Log Power Spectrogram</h4>
         <img src="spectrogram/Example_Chopper.svg" alt="Log power spectrogram of noisy audio">
         <div class="caption">Log power spectrogram of noisy audio</div>
      </div>
      <!-- ********************************************* -->
      <h2>4. Static-radio noise (-5 dB)</h2>
      <div class="column">
         <h3>Clean Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Static-radio/clean.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Actual Transcript</b><br>
            ZEHN E L W ZWO EINS VON FLORIAN NULL G W D U K EINS KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Static-radio/clean.svg" alt="Waveform of clean audio">
         <div class="caption">Waveform of clean audio</div>
      </div>
      <div class="column">
         <h3>Noisy Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Static-radio/noisy.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            ZEHN E L W ZWO EINS VON FLORIAN DORTMUND NULL G W D U K EINS KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Static-radio/noisy.svg" alt="Waveform of noisy audio">
         <div class="caption">Waveform of noisy audio</div>
      </div>
      <div class="column">
         <h3>Enhanced Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Static-radio/enhanced.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            ZEHN E L W ZWO EINS VON FLORIAN DORTMUND NULL G W D U K EINS KOMMEN
         </div>
         <img class="spectrogram" src="waveform/Static-radio/enhanced.svg" alt="Waveform of enhanced audio">
         <div class="caption">Waveform of enhanced audio</div>
      </div>
      <div class="spectrogram">
         <h4>Log Power Spectrogram</h4>
         <img src="spectrogram/Example_Static-radio.svg" alt="Log power spectrogram of noisy audio">
         <div class="caption">Log power spectrogram of noisy audio</div>
      </div>
      <!-- ********************************************* -->
      <h2>5. Breathing noise (-5 dB)</h2>
      <div class="column">
         <h3>Clean Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Breathing/clean.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Actual Transcript</b><br>
            FÜR EI SIE EINSATZAUFTRAG ERKUNDUNG DER HALLE INNEN MIT BODENROBOTISCHEN SYSTEMEN
         </div>
         <img class="spectrogram" src="waveform/Breathing/clean.svg" alt="Waveform of clean audio">
         <div class="caption">Waveform of clean audio</div>
      </div>
      <div class="column">
         <h3>Noisy Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Breathing/noisy.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            JA FÜR SIE EINSATZAUFTRAG ERKUNDUNG DER HALLE IN MIT BODENROBOTISCHEN SYSTEMEN
         </div>
         <img class="spectrogram" src="waveform/Breathing/noisy.svg" alt="Waveform of noisy audio">
         <div class="caption">Waveform of noisy audio</div>
      </div>
      <div class="column">
         <h3>Enhanced Audio</h3>
         <div class="audio">
            <audio controls>
               <source src="audio_files/Breathing/enhanced.wav" type="audio/wav">
            </audio>
         </div>
         <div class="textbox">
            <b>Predicted Transcript</b><br>
            JA FÜR SIE EINSATZAUFTRAG ERKUNDUNG DER HALLE IN MIT BODENROBOTISCHEN SYSTEMEN
         </div>
         <img class="spectrogram" src="waveform/Breathing/enhanced.svg" alt="Waveform of enhanced audio">
         <div class="caption">Waveform of enhanced audio</div>
      </div>
      <div class="spectrogram">
         <h4>Log Power Spectrogram</h4>
         <img src="spectrogram/Example_Breathing.svg" alt="Log power spectrogram of noisy audio">
         <div class="caption">Log power spectrogram of noisy audio</div>
      </div>
      <hr>
      <div class="acknowledgement" style="font-size: 24px; font-style: italic;">
         <h2>Acknowledgement</h2>
         <p>Special thanks to my supervisors and advisors for their guidance and support throughout the project. This work was supported under the project A-DRZ: Setting up the German Rescue Robotics Center and funded by the German Ministry of Education and Research (BMBF), grant No. I3N14856.</p>
      </div>
