# Week 5 Progress — GenPT: AI Images and Embodied Interaction

**Session:** Wednesday, September 30, 2026  
**Project:** GenPT / TouchDesigner  
**Working environment:** Mac, TouchDesigner 2025.32280, connected through MCP  
**Open project at documentation time:** `GenPT_AI_Image_GeneratorV2.1.toe`

## Overview

This week’s session extended the existing GenPT-inspired feedback and pixel-weaving project with actual AI image generation and webcam gesture control. We replaced six static image inputs with two custom OpenAI image-generator components, then developed an interaction in which closing a hand into a fist requests a randomly selected religious artifact or architectural subject.

The main progress was in how the system is used: a participant can initiate image creation through a physical action, while TouchDesigner continues to blend, stretch, and remember the resulting images. We refined the interaction together through testing, moving from pinch to prayer to fist as recognition problems became apparent.

![TouchDesigner workspace and final Hand Gesture controls](/Users/retochen/Documents/Codex/2026-09-21/touchdesigner-plugin-touchdesigner-touchdesigner-openai-check-2/outputs/week5_assets/01_td_workspace.png)

*Figure 1. The current workspace, existing feedback/weaving network, and Hand Gesture controls. The selected subject is “Celestial ceiling painting.” Preview and generation triggering were both switched off at capture time.*

## 1. Reconnecting TouchDesigner

The first MCP check failed because the configured TouchDesigner server at `127.0.0.1:9981` refused the connection. A second check succeeded and confirmed TouchDesigner 2025.32280 on macOS 15.7.4, with API server 1.6.0 and MCP server 2.1.0.

This restored direct access to the live project for inspecting operators, configuring parameters, wiring nodes, and checking output. Project edits were made in the currently open file. During development, the assistant did not save or reopen a project. By documentation time, the open file had changed from `GenPT_AI_Image_GeneratorV2.toe` to `GenPT_AI_Image_GeneratorV2.1.toe`; the workspace screenshot also shows a prior successful save notification. Documentation capture itself did not save the project.

## 2. Replacing six static sources with two AI generators

Two custom components were built inside `/project1/audio_image_morph`:

- `ai_generator_1`
- `ai_generator_2`

The six Movie File In TOPs were removed. The existing six fitting branches were retained and connected alternately to the two generator outputs. This preserved the downstream blending and transformation network while reducing the number of independent image sources to two.

Each component sends a prompt to OpenAI and exposes the returned image through an output TOP. The API request runs in a background thread. TouchDesigner receives the result on its main thread and fades it into the current image, rather than blocking the visual network while the service responds.

### Generator controls

| Control | Function |
| --- | --- |
| Prompt | Text instruction for the next image |
| Model | Configured as `gpt-image-1` |
| Quality | Initially medium |
| API Key File | Selects a local plain-text file containing the key |
| Use Current Frame | Includes a snapshot of the Reference TOP in the request |
| Reference TOP | Initially points to `/project1/out1` |
| Generate Image | Sends a manual request |
| Auto Generate | Enables repeated paid requests; initially off |
| Seconds Between Requests | Initially 60, with a minimum interval enforced in code |
| Image Fade Seconds | Initially 3 seconds |
| Status / Images Received | Shows request progress and completed-image count |

Generated images are center-cropped and resized to a consistent **1920 × 1080** TOP output. Alpha is forced opaque. This maintains the canvas dimensions as new content arrives. The configured API image size is **1536 × 1024**, so the 1920 × 1080 output is a fitted/upscaled image, not native AI generation at that resolution.

### Implementation refinements

Several original image paths were missing and produced warnings and a black initial preview. We recovered the remaining available source into memory as a starting image. That held source was explicitly distinguished from a newly generated AI result.

The first Script TOP upload approach also produced a black output. We corrected it by uploading normalized floating-point pixel arrays directly outside the Script TOP’s cook callback. OpenCV handles image encoding, decoding, and fitting; Pillow was unavailable in TouchDesigner’s Python environment.

Returned images stay in memory. We did not create a generated-image archive during development. The PNGs accompanying this document were explicitly exported later for documentation.

![Generator 1 image before the feedback and weaving effects](/Users/retochen/Documents/Codex/2026-09-21/touchdesigner-plugin-touchdesigner-touchdesigner-openai-check-2/outputs/week5_assets/03_ai_source.png)

*Figure 2. Generator 1’s current source: the selected “Celestial ceiling painting” prompt produced a gold-framed celestial composition. This capture shows the normalized source TOP before the downstream feedback/weaving chain.*

## 3. API setup and practical use

We worked through creating an OpenAI API key, copying it into a plain-text file using TextEdit, and selecting that file on the generator components. The secret itself was not placed in this document or printed during debugging.

Generator 2 initially did not respond because its API Key File was not configured. We connected it to the same working key file as generator 1. Another interaction detail became clear: editing Prompt does not itself send a request; generation starts through Generate Image, Auto Generate, or the gesture trigger.

We also discussed budgeting. The session estimate used medium-quality `gpt-image-1` landscape output at approximately $0.063 per image, excluding prompt/reference input charges. For an assumed eight weeks at three hours per week, two generators making one image per minute would produce approximately 2,880 images and $181 in image-output charges. Slower intervals substantially reduce this total. These were planning estimates, not a measurement of actual spending.

## 4. Exploring inputs beyond audio

We brainstormed body movement, hand gestures, drawing, physical objects, distance/presence, and physical controls as alternatives to sound-driven interaction.

References discussed included:

- [MIRAO — Aether Immersive](https://aetherimmersive.com/work/mirao/): camera/depth input and participant prompts drive AI visuals in TouchDesigner.
- [AI EEG Study — Virgil Puiac](https://www.puiac.art/installations-mapping/ai-eeg-study): EEG readings are mapped to changes in generated nature imagery using StreamDiffusion in TouchDesigner.
- [ThoughtDiffusion](https://alife-robotics.co.jp/LP/2025/OS24-2.htm): an installation exploring EEG and body movement with generated imagery.
- [Diffusion TV — Sihwa Park](https://arxiv.org/abs/2609.05404): physical television controls provide an interaction reference; TouchDesigner involvement was not verified.

For this project, we chose the Mac webcam because it was already available and required no additional sensor hardware.

**Scope distinction:** the webcam gesture now initiates AI generation. We did not remove the existing microphone network or comprehensively replace every audio-driven visual parameter. Generation control and continuous visual modulation are separate layers.

## 5. Developing the gesture through three versions

A new component, `/project1/webcam_gesture`, uses the built-in MacBook Pro camera and local hand detection.

TouchDesigner did not have the MediaPipe Python package installed, but it did have OpenCV. We used the OpenCV Zoo versions of the MediaPipe palm and hand-landmark models. Their weights were downloaded into temporary storage and embedded in the component’s storage. Hand detection does not require uploading camera frames to OpenAI.

| Version | Intended action | What testing taught us |
| --- | --- | --- |
| Pinch | Hold thumb and index together | Recognition was intermittent; detection thresholds and hold timing needed adjustment |
| Prayer | Bring two palms together, fingers upward | One hand often hides the other, making a two-hand requirement difficult |
| Fist — final version | Close either hand | Requires only one visible hand and is easier to perform |

### Pinch refinements

The first interaction held a pinch for one second and alternated between the two generators. We added a release-to-rearm condition and a 30-second cooldown.

When the interaction seemed unresponsive, the logs already showed successful gesture requests. This helped separate **gesture recognition**, **request submission**, and **image arrival** rather than treating all three as one failure. We made recognition more forgiving and briefly shortened the pinch hold to 0.7 seconds.

Another issue was scheduling: frame-end callbacks did not consistently run while the timeline was paused. A recurring wall-clock heartbeat was added so detection continues independently of timeline playback.

### Prayer experiment

Prayer recognition looked for two close hands with extended fingers pointing upward. It approximated the pose through landmarks; it could not verify physical palm contact.

The user found this too difficult to detect. Occlusion was the central problem: a visually clear praying gesture for a person can conceal landmarks needed by the detector. We replaced it rather than requiring a more elaborate camera setup.

### Final fist behavior

The final classifier checks curled fingers on either detected hand. At least three fingers must meet the curl conditions; thumb position is optional. The participant holds the fist for **one second**, then opens the hand to rearm. A **30-second cooldown** prevents continuous or rapidly repeated API requests.

Synthetic checks verified that a modeled fist is accepted, an open hand and absent hand are rejected, and either of two visible hands can supply the fist. These checks validate the logic, not recognition reliability in every real camera pose.

## 6. Preview, markers, and visible feedback

We added a **Camera Preview** toggle to the Hand Gesture tab. It opens or closes the viewer while detection can continue independently.

The initial preview used the processed detection frame and visibly lagged behind the camera. We changed it to a direct camera feed, then composited a separate transparent overlay above that feed.

The overlay includes:

- Colored landmark dots and hand connections.
- Current gesture status.
- Number of hands detected.
- Gesture request count.
- A hold-progress bar.

This lets the participant distinguish “the camera sees me” from “the system recognizes my hand” and “the gesture has triggered a request.”

The footage is no longer held until inference finishes. Markers still update at the detector’s rate, and ordinary camera/display latency remains. We did not establish a measured zero-latency performance claim.

![Webcam preview with live status overlay](/Users/retochen/Documents/Codex/2026-09-21/touchdesigner-plugin-touchdesigner-touchdesigner-openai-check-2/outputs/week5_assets/04_gesture_preview.png)

*Figure 3. Captured webcam preview with the status banner, hand count, request count, and progress-bar area. At this exact frame the detector reported zero hands, so no landmark dots are visible. The capture documents the overlay and also demonstrates that a hand near the face is not guaranteed to be recognized.*

## 7. Random religious subjects: the final interaction

The fist action was then changed to trigger **ai_generator_1 only**. Each accepted gesture selects a religious artifact or architectural subject from the supplied collection: **85 entries**, consisting of Gothic rose window plus the 84 numbered items.

The collection includes objects and spaces from multiple traditions: a Catholic monstrance, Tibetan prayer wheel, Shinto torii gate, Daoist incense burner, Islamic muqarnas ceiling, Hindu gopuram, Orthodox iconostasis, Jewish menorah, and many other subjects.

Selection is random and excludes the immediately previous subject. It is not a shuffled deck: a subject can recur later.

The generated prompt follows this template:

> A detailed artistic depiction of [selected subject], focusing on its distinctive form, materials, and ornamentation. Atmospheric lighting, richly layered texture, landscape composition.

The selected item is shown in **Selected Religious Subject**, and the complete prompt appears in generator 1. A busy generator does not receive another request or have its prompt replaced by a new gesture.

Use Current Frame remains the generator’s own option. When enabled, it sends the configured artwork TOP with the prompt. This is different from sending the webcam image: the webcam’s role is local gesture recognition.

### Artistic implication

The interaction now links a bodily action to a collection of sacred objects and spaces. The audience initiates the encounter, while random selection introduces an element of chance. TouchDesigner then folds the generated image into its existing visual memory and stretched-pixel structure.

The imagery is an AI interpretation of the subject. The session did not establish historical, architectural, or religious accuracy for generated depictions.

## 8. How the current system fits together

```text
Mac webcam
  → local palm detection and hand landmarks
  → closed-fist recognition
  → one-second hold + release condition + cooldown
  → random religious subject
  → generator 1 prompt
  → asynchronous OpenAI image request
  → opaque 1920 × 1080 output and gradual image fade
  → existing blend / feedback / stretched-pixel weaving
  → final artwork TOP
```

Generator 2 remains available as another image source with its own prompt and manual controls. The final gesture no longer alternates between it and generator 1.

![Final artwork with feedback and woven stretched pixels](/Users/retochen/Documents/Codex/2026-09-21/touchdesigner-plugin-touchdesigner-touchdesigner-openai-check-2/outputs/week5_assets/02_final_artwork.png)

*Figure 4. Final TouchDesigner output captured during documentation. The celestial source has been layered, warped, and interrupted by horizontal stretched-pixel rectangles. Comparing Figures 2 and 4 shows the contribution of the native effects beyond the AI-generated source.*

## 9. State recorded at the end of the session

| Item | Recorded value |
| --- | --- |
| Open file | GenPT_AI_Image_GeneratorV2.1.toe |
| Final output | 1920 × 1080 |
| Webcam / preview | 640 × 360 |
| Gesture | Closed fist on either hand |
| Hold / cooldown | 1 second / 30 seconds |
| Target | Generator 1 |
| Subject collection | 85 entries |
| Latest selected subject | Celestial ceiling painting |
| Gesture request counter | 11 |
| Generator 1 images received | 9 |
| Generator 2 images received | 5 |
| Generator 1 Use Current Frame | Off |
| Generator 2 Use Current Frame | Off |
| Camera Preview toggle | Off |
| Gesture Trigger Enabled | Off |

These counters are the live component values, not an independently audited total of API usage. They include interaction across the session’s evolving versions.

The final project error check reported **zero operator errors and zero warnings**. This does not prove gesture recognition works for every participant or pose. The documentation capture confirmed both the AI source and processed final artwork were visible.

## 10. Remaining work

- Test fist recognition with varied hand angles, lighting, distances, and participants.
- Make triggered/requesting/received states persist clearly enough for an audience to follow.
- Evaluate whether the remaining microphone-driven effects should be replaced by hand movement or another input.
- Refine the prompt’s aesthetic direction and review how faithfully different sacred subjects are represented.
- Decide how much previous-frame influence to use: reference-based generation can preserve continuity but may also carry unwanted forms into the next subject.
- Consider a deliberate image archive and subject/prompt history if later research needs reproducibility.

## Reflection

Week 5 brought AI image creation into the native GenPT feedback workflow and made it accessible through a bodily gesture. The collaborative refinements addressed both technical behavior and the participant’s ability to understand the system: a reliable request schedule, consistent image dimensions, visible detector status, controlled API triggering, and a simpler gesture.

The move from prayer to fist was especially instructive. A meaningful or expressive gesture still has to be legible to the sensor. The final design balances that practical requirement with a subject collection that gives the generated imagery a more specific conceptual direction.

---

**Documentation assets:** four captures are stored beside this document in `week5_assets/`. The document uses absolute image paths for local preview; adjust those paths if moving the folder to another computer. The webcam capture includes the participant’s image. No API key is included. Screenshots were exported for this log; the TouchDesigner project was not saved as part of documentation.

