# PoseTrack

**Body keypoint detection that runs entirely in your browser.**

Drop in a photo — or switch on your webcam — and PoseTrack finds 33 points on the
human body, draws the skeleton, and measures the angle at every major joint.

No server, no upload, no account. Your photos and video never leave your device.

---

## What it does

- **33 keypoints** — face, shoulders, elbows, wrists, hands, hips, knees, ankles, feet
- **Live webcam mode** — real-time tracking at video frame rate
- **Joint angles** — elbows, shoulders, hips and knees, measured in degrees
- **Left/right colour coding** — left side amber, right side blue, so you can read it instantly
- **Three model sizes** — Lite for speed, Heavy for accuracy
- **Export** — annotated PNG, or full JSON with every coordinate, confidence and angle

## Who it's for

- **Physios and trainers** — checking form, range of motion, symmetry
- **Sports coaches** — comparing technique frame by frame
- **Students and developers** — a working reference for browser-based pose estimation
- **Anyone curious** about how computers see the human body

---

## Important: it's trained on humans

The model behind PoseTrack ([MediaPipe BlazePose](https://developers.google.com/mediapipe))
was trained on photographs of **people**. Point it at a horse, a dog or a bird and it will
produce confident nonsense — it will try to force a human skeleton onto whatever it sees.

**Animal pose tracking is a different job.** It needs a model trained on that specific
animal, which means:

1. Collecting a few hundred photos of the animal
2. Hand-labelling every keypoint in every photo (the slow part)
3. Training a model with a tool like [DeepLabCut](https://deeplabcut.github.io/) or [SLEAP](https://sleap.ai/)
4. Exporting it to a format a browser can run (TensorFlow.js or ONNX)

That's a real project, not a config change. This site is the working foundation
it would be built on.

---

## How it works under the hood

| Piece | What it is |
|---|---|
| Model | MediaPipe Pose Landmarker (BlazePose), `lite` / `full` / `heavy` |
| Runtime | WebAssembly + WebGL, loaded from jsDelivr |
| Detection | `detect()` for photos, `detectForVideo()` for the webcam |
| Angles | Dot product between the two bones meeting at each joint |
| Privacy | Everything runs client-side — no image ever leaves the browser |

The first visit downloads the model (a few MB). After that the browser caches it
and start-up is instant.

---

## Running it locally

Single HTML file, no build step. It **does** need a real web server — browsers
block JavaScript modules on `file://` URLs.

```bash
git clone https://github.com/jawadhasuna/Pose-Track
cd Pose-Track
python -m http.server 8000
```

Then open <http://localhost:8000>.

## Deploying

Any static host works. This repo is set up for [Vercel](https://vercel.com) —
import it and deploy, no configuration needed.

---

## Origin

Ported from a set of Python + Tkinter desktop prototypes (`pose2.py`–`pose5.py`)
that compared MediaPipe, OpenPose, MMPose, DeepLabCut and SLEAP. MediaPipe was the
only one that ran without a hand-trained model, and it happens to ship an official
browser build — so that's the one that made it to the web.

## Licence

MIT
