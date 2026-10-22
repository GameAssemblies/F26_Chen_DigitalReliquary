
## Weekly Progress — October 2026

![](../Images/Screenshot%202026-10-21%20at%205.55.27%20PM.png)

This week, the Ritual Engine was integrated into the existing **TouchDesigner Particle Universe** project. The integration preserves the original particle network and adds a modular control system for movement analysis, ritual-stage progression, synchronized audio, visual modulation, testing, and monitoring.

The current working project is:

- `Particle_Universe_Ritual_Engine_v3_movement.1.toe`

The reusable engine component is:

- `Ritual_Engine_v3_movement.tox`

## What Is Currently in the System

### Modular Ritual Engine

The new system is contained in `/project1/ritual_engine` and organized into dedicated areas:

- **Vision** — camera input, movement analysis, calibration, and stage logic
- **Controls** — live/manual operation and testing controls
- **Audio** — synchronized music stems and stage-based mixing
- **HUD** — live system status and progression feedback
- **Output** — final composited program image

This structure keeps the Ritual Engine separate from the Particle Universe network and makes it possible to reuse or replace either system independently.

### Four-Stage Progression

The experience progresses through four retained stages:

| Stage | Name | Audio state |
|---:|---|---|
| 0 | Arrival | Bass |
| 1 | Gathering | Bass + kick/hats |
| 2 | Synchronization | Bass + kick/hats + pressure texture |
| 3 | Threshold | Full mix + deep bell |

Progression is driven by an accumulated **charge** value. Movement increases charge, inactivity releases it, and sustained behavior moves the system through the stages.

Stage changes use boundary margins and timed entry/exit holds. This prevents rapid flickering when charge remains close to a stage boundary.

### Camera-Based Movement Response

The current engine analyzes changes in the camera image. It does not yet identify individual bodies or poses.

The retained movement system includes:

- A selectable camera analysis region
- Stillness and normal-movement calibration
- Exposure-change suppression
- Local movement weighting, so a participant does not need to fill the frame
- Time-based analysis and normalization
- Separate attack and release smoothing
- A pause grace period before charge begins to fall
- Automatic rebaselining after dropped or delayed camera frames
- Rhythm reinforcement based on recent repeated movement intervals

The delivered project contains safe default sensitivity values. The room and participant movement should still be calibrated at the installation location.

### Audio System

Four 124 BPM, four-bar stems are retained and synchronized to a shared playback clock:

- Bass
- Kick and hats
- Pressure texture
- Deep bell

The stems remain aligned while running. Stage-based layer changes are applied on beat boundaries so musical transitions do not occur at arbitrary points.

### Particle Universe Integration

The original visual network remains intact. The Ritual Engine sends control values into three existing visual parameters:

- Particle noise amplitude responds to movement and rhythm
- Particle scale responds to stage and within-stage progress
- Output brightness responds to charge, beat pulse, and the active audio stage

The amount of visual modulation can be adjusted with **Visual Response**. Setting it to zero returns these parameters to their original values.

### HUD and Program Output

The Particle Universe image feeds the Ritual Engine HUD, and the composite becomes the final program output:

`/project1/ritual_engine/output/program_out`

The HUD reports the current stage, charge, movement, rhythm, calibration state, and pending beat-quantized audio changes. The final output is configured at 1920 × 1080.

### Manual Testing and Adjustment Controls

Select `/project1/ritual_engine` and press **P** to access the custom controls.

The retained controls cover:

- Live camera or manual test mode
- Manual selection of stages 0–3
- Stillness calibration
- Normal-movement calibration
- Camera analysis area
- Movement sensitivity
- Motion attack and release timing
- Charge rise and cooldown speeds
- Pause tolerance
- Stage boundary margin and entry/exit holds
- Rhythm influence
- Visual response amount
- Audio stem resynchronization
- Charge reset

The detailed adjustment procedure is documented in `MOTION_V3_GUIDE.md`.

## Validation Completed

The retained system was checked in the live TouchDesigner project for:

- Camera input and movement callbacks
- Manual stage changes from 0 through 3
- Correct audio layers at every stage
- Beat-quantized audio transitions
- Synchronized stem playback
- Charge progression and cooldown
- Stable stage boundaries
- Movement smoothing across different analysis rates
- Uniform brightness-change rejection
- Calibration success and rejection behavior
- HUD stage and progress display
- Final program output
- TouchDesigner operator errors and warnings

The final live audit reported no operator errors or warnings. Real-world sensitivity must still be calibrated in the final camera position and lighting conditions.

## Current Limitations

- Movement is measured from image differences rather than skeletal pose data.
- Multiple people currently contribute to one combined movement signal.
- The rhythm value measures regular movement intervals; it does not yet measure synchronization between people or synchronization to musical beats.
- Camera movement, strong shadows, and nonuniform lighting changes can affect the movement signal.
- Pose tracking and two-person identity/calibration were evaluated as future improvements but are not part of this retained build.

## Main Files

| File | Purpose |
|---|---|
| `Particle_Universe_Ritual_Engine_v3_movement.1.toe` | Current integrated project |
| `Ritual_Engine_v3_movement.tox` | Reusable Ritual Engine component |
| `MOTION_V3_GUIDE.md` | Setup, calibration, and tuning guide |
| `motion_v3_tests.json` | Synthetic movement-engine checks |
| `motion_v3_live_tests.json` | Live callback and stage checks |
| `Particle_Universe_before_Ritual.toe` | Pre-integration backup |
| `Particle_Universe_before_motion_upgrade.toe` | Backup before the v3 movement upgrade |

## Recommended Next Step

Calibrate the retained movement system in the intended space and test it with real participants. A later Mac-compatible pose-tracking layer can add separate participant IDs, individual movement energy, distance, posture, and two-person synchrony without replacing the current stage, audio, HUD, or particle-control architecture.
