# Speed Pitch

A standalone, mobile-first motion synthesizer. Move your phone through the air to make sound: slower motion produces a deep tone; faster motion produces a higher tone. Hold still and the instrument goes quiet.

## Run it on your phone

1. Serve `index.html` from an HTTPS static host. No build, dependencies, or backend are needed.
2. Open the page in Safari on iPhone or Chrome on Android.
3. Tap **Start instrument** and grant motion sensor permission if prompted.
4. Hold the phone still for the brief calibration, then move it gently. Adjust media volume as needed.
5. Tap **Stop instrument** to silence it.

To host with GitHub Pages, open this repository's Settings → Pages, choose **Deploy from a branch**, and select `main` and `/ (root)`. Once published, use the HTTPS URL provided there. Opening an HTML file directly may not allow motion sensors on some devices.

On desktop, **Test tone** opens a simulated speed slider so you can hear the full range without motion sensors.

## Controls

- **Sensitivity:** how quickly pitch rises with movement.
- **Stillness deadband:** how much small motion is ignored. Raise it if your phone sounds when resting.
- **Deep pitch / High pitch:** choose the endpoints (55 Hz and 1800 Hz by default).
- **Volume:** controls the synthesizer's output level.
- **Tone:** triangle, sine, sawtooth or square wave.
- **Recalibrate:** keep the phone still to reset the motion baseline.

## Implementation

The app uses DeviceMotion events, preferring gravity-free acceleration. If only acceleration including gravity is available, it estimates and subtracts gravity with a low-pass filter. It integrates the corrected acceleration over time with a decaying velocity estimate to limit drift, adds a small rotation-derived speed component, and maps the smoothed result exponentially onto the chosen pitch range. A stillness timeout and adjustable deadband stop the tone; Web Audio ramps soften pitch and volume changes.

Motion sensors do not measure absolute velocity. These estimates work best with short handheld sweeps, not constant-speed travel in a vehicle. Sensor availability and behavior vary by browser and device.

Audio and motion processing run locally. No data is uploaded, and no GPS or microphone access is requested. The instrument stops automatically when the page is hidden.
