Walk Assist

A voice-first walking assistant that runs in a phone browser. It uses the camera to warn about people, objects, walls and stairs, and it speaks and vibrates in English, Hindi or Kannada.

Live demo: https://rohankawate.github.io/walk-assist-/(Open it on a phone, tap Start walking and allow the camera. No camera? Tap Try demo.)

Prototype only. This project has not been tested with visually impaired users. It is meant to add to a white cane or a guide, never to replace one. Distances and wall or stair detection are estimates.

<!-- Add 2 or 3 screenshots here, for example: ![Dashboard](screenshots/dashboard.png) -->
Why I built it

A cane finds things at ground level, but it does not say what an object is, which way it is moving, or warn about things at head height. Many assistive tools need extra hardware or only speak English. Walk Assist uses a phone people already own, runs with no account, and speaks in local languages.

Features
Feature	What it does
Object detection	Finds people, vehicles, furniture and more (COCO-SSD).
Left / ahead / right zones	Tells you where the obstacle is and which way to step.
Danger list	Vehicles trigger an earlier, stronger warning.
Distance estimate	Uses the known height of a person or vehicle to estimate meters.
Approaching detection	A growing box over a few frames gives "person approaching".
Wall detection	A depth model (Depth Anything V2 small) flags walls and unknown obstacles.
Stairs and drop-offs	Looks for a sudden step in the floor profile ahead. Experimental.
Vibration patterns	Gentle tap (nearby), fast pulses (danger), one long buzz (stop).
Voice commands	Say "start", "stop" or "repeat". Also "hindi", "kannada", "english".
Three languages	Alerts and commands in English, हिन्दी and ಕನ್ನಡ.
Low-light warning	Says so when the picture is too dark to trust.
SOS	Two taps open WhatsApp with a Google Maps link to your location.
Demo mode	Scripted scenes, no camera needed.
Test log	Exports every alert as a CSV file for results tables.
Settings saved	Language, toggles, sensitivity, speech speed and volume are remembered.
Installable	Works as a home-screen app (PWA).
How it works
Camera frame
   ├── COCO-SSD (TensorFlow.js) ──► objects + boxes
   │        └── tracking over frames ──► approaching
   │        └── known heights / box position ──► distance
   └── Depth Anything V2 small (Transformers.js, in a worker)
            └── nearness per zone ──► walls
            └── floor profile ahead ──► stairs / drop-offs
                         │
              decision logic (zones, levels, translations)
                         │
        spoken message  ·  vibration pattern  ·  dashboard

Everything runs in the browser. The camera video never leaves the phone. Only the browser's speech recognition (voice commands) uses an online speech service.

Tech stack
HTML, CSS and JavaScript (no framework, no backend)
TensorFlow.js and the COCO-SSD model
Transformers.js with onnx-community/depth-anything-v2-small
Web Speech API (speech synthesis and recognition), Vibration API, Geolocation API
Service worker and web manifest (PWA)
Run it locally

Camera access needs https:// or localhost, so do not open the file by double-clicking.

Clone or download this repo.
Open the folder in VS Code and start the Live Server extension.
Open the local address on your computer, or deploy it (below) to test on a phone.
Deploy for free (GitHub Pages)
Upload all files in this repo to a public GitHub repository.
Go to Settings > Pages, choose Deploy from a branch, branch main, folder / (root).
Open https:https://rohankawate.github.io/walk-assist-/ on your phone.
Using it
Hang the phone on a lanyard or chest pocket, camera facing forward, and use one earphone so you can still hear traffic.
Tap Start walking, or say "start" once voice commands are on.
In Settings, set the language, add an emergency contact (with country code, for example +91...), and adjust speech speed, volume and Wall sensitivity.
Tuning
SURE in the script: below this confidence, the app says "obstacle" instead of a possibly wrong name.
THRESHOLDS, KNOWN_HEIGHT, APPROACH_RATIO: how close counts as near, which objects have distance, and how fast growth means approaching.
Wall sensitivity slider: moves the wall and stair limits. Settings shows the live depth numbers to help you tune.
Known limitations
COCO-SSD has 80 classes. It knows nothing about walls, doors, poles or stairs, and it sometimes mislabels a large flat surface (a wall was once called a refrigerator).
Depth Anything gives relative depth, not meters. Wall and stair thresholds are tuned by hand and need testing in your own environment.
Distance is estimated for a few object types and assumes a typical phone field of view. A person who is only partly in the frame gives a poor estimate.
Detection gets worse in low light, and the depth model is heavy on older phones.
Hindi and Kannada voices depend on the phone. Translations should be checked by native speakers.
Not tested with blind or low-vision users.
Future work
Test with visually impaired users and organizations that work with them.
Train or fine-tune a model for walls, doors, poles and stairs.
Add GPS walking directions and reading of signs (OCR).
Compare COCO-SSD alone against COCO-SSD with depth, with measured results.
Credits
TensorFlow.js COCO-SSD
Depth Anything V2 and Transformers.js

Built by ROHAN, as a second-year AIML student project, 2026.
