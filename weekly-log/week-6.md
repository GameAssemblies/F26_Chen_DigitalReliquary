# Digital Shrine — Concept Development + Action Items
**Date:** October 5, 2026  
**Course / Context:** DTech Independent Study / TouchDesigner exploration  
**Working project title:** *Digital Shrine*  
**Previous working title:** *Digital Reliquary*

---

## 1. Project Direction Recap

The project began as an exploration of how sensations associated with ritual and sacred physical spaces might be translated into a large-scale digital media experience.

Initial interests included:

- sacred architecture and ritual spaces such as churches and temples
- the feeling of entering a space with a different atmosphere or “aura”
- using human gesture, sound, symbols, and AI as inputs or mediators
- creating a participatory ritual between one human and one machine
- exploring a nonhuman social / relational space between participant and machine
- asking what conditions might move someone toward heightened attention, awe, presence, or a sense of transcendence

Early technical exercises included:

1. audio-reactive visuals that alter pixels and switch image outputs
2. AI image generation through the OpenAI API
3. webcam / computer vision experiments that recognize gestures such as praying hands

The key conceptual shift from today’s discussion was to stop treating these as separate “features” and instead define a single experiential spine that every technical component must support.

---

## 2. Centralized Concept Spine

### Current Core Question

> How can a large-scale responsive screen use ritual-like pacing, bodily attention, and AI transformation to create a temporary sense of sacred presence between a person and a machine?

### Current Design Goal

The participant should clearly understand that the system is responding to them, while avoiding the feeling that they are simply operating a conventional interface.

The system should behave more like a **digital shrine** than a tool.

---

## 3. Key Decisions From the Mind-Map Narrowing

### Concept

- **Primary focus:** ritual experience
- **Machine role:** sacred object / shrine rather than companion
- **Interaction style:** learnable reciprocity rather than total ambiguity
- **Overall structure:** continuous evolving state rather than clearly separated numbered acts
- **Desired reaction:** “That felt strangely sacred / meditative / intense,” while the participant still understands that the piece was interactive

### Input

- **Primary input:** webcam / computer vision
- **Primary interaction:** body / gesture
- **Sound:** secondary atmospheric input rather than equal-weight interaction
- Avoid a hidden vocabulary of “correct” religious gestures
- Prefer open-ended bodily qualities such as:
  - stillness
  - speed
  - openness
  - symmetry
  - proximity
  - movement intensity

### AI

- **AI role:** interpret / transform the participant’s literal body image
- The participant’s body becomes source material for generative imagery
- AI should not behave like a chatbot, oracle, or conversational personality
- AI generation should support the ritual experience rather than exist as a standalone spectacle

### Visual Direction

- abstract / atmospheric
- post-religious rather than tied to one doctrine
- sacredness should come from:
  - pacing
  - scale
  - darkness
  - sound
  - attention
  - visual transformation
  - ambiguity
- avoid generic “religious AI imagery” and obvious symbolic collage

### Ending

- no archive
- no saved relic
- no persistent participant memory
- the experience is ephemeral
- if no new input occurs, the system gradually closes itself and returns to dormancy

---

## 4. Current Experience Structure

### Experience Spine

**Dormant**  
→ **Approach**  
→ **Acknowledgement**  
→ **Attunement**  
→ **Deepening**  
→ **Manifestation**  
→ **Peak**  
→ **Dissolution**  
→ **Dormant**

### 1. Dormant

The screen is quiet, minimal, restrained, and waiting.

The goal is to establish the piece as something present but not actively performing.

### 2. Approach

A participant enters the sensing range.

The system begins to recognize human presence.

### 3. Acknowledgement

The shrine gives an early, understandable response.

This stage communicates:

> “The system can see me.”

Early feedback should be clear enough to teach interactivity.

### 4. Attunement

The participant experiments naturally with:

- body position
- distance
- movement
- stillness
- posture

The participant begins to learn:

> “My behavior affects the environment.”

### 5. Deepening

As the participant remains with the piece:

- interaction becomes slower
- cause-and-effect becomes less literal
- visual transformation becomes richer
- the system becomes more mysterious

The participant should still feel agency, but no longer feel complete control.

### 6. Manifestation

The participant’s captured body / silhouette becomes source material for AI generation.

The system transforms the human figure into increasingly abstract visual material.

### 7. Peak

The strongest moment combines:

- visual scale
- motion
- density
- sound
- AI-generated transformation
- partial recognition of the participant’s own body

The ideal reaction becomes:

> “I recognize that this came from me, but I do not fully understand what it has become.”

### 8. Dissolution

If no meaningful new input occurs for a defined amount of time:

- intensity decreases
- sound recedes
- imagery dissolves
- traces of the participant disappear
- the shrine returns to dormancy

Nothing is stored.

---

## 5. Conceptual Rules

### Shrine, Not Tool

The participant influences the system but does not fully control it.

Avoid direct mappings such as:

> move hand left → image moves left

The system should interpret rather than obey.

### Presence, Not Task Completion

Progress comes primarily from sustained attention and presence, not from performing a correct gesture.

### Legible → Mysterious

Early interaction should clearly communicate responsiveness.

Later interaction should become slower, more interpretive, and less predictable.

### Body as Material

The participant does not merely control parameters.

Their body becomes visual material that the system transforms.

### Ephemeral Encounter

The interaction only exists while attention is maintained.

Once the participant leaves or disengages, the system erases the encounter.

### Post-Religious

The work can be inspired by sacred spaces and ritual without prescribing a religion.

The goal is to evoke qualities such as:

- reverence
- slowness
- awe
- attention
- liminality
- transformation

---

## 6. What We Are Cutting

To keep the project focused:

- no seven-act explicit ceremony
- no catalog of correct prayer gestures
- no equal-weight voice interaction
- no collage of explicit religious iconography
- no persistent participant archive
- no digital relic / memory system
- no AI chatbot
- no oracle behavior
- no conversational AI personality
- no feature unless it visibly improves the ritual experience

---

# 7. This Week’s TouchDesigner Exploration

## Prototype 01 — Attunement

### Core Question

> Can the screen feel like it is gradually becoming aware of me, rather than simply reacting to my movement?

Do **not** build the whole project this week.

Focus on proving the basic ritual relationship.

---

## 8. Three Prototype States

### State 1 — Dormant

- dark / minimal screen
- subtle movement only
- no obvious “UI”
- system feels inactive but present

### State 2 — Attunement

- webcam detects participant
- shrine gives restrained feedback
- body affects visual environment
- early reaction should be understandable

### State 3 — Deepening

- sustained engagement increases visual transformation
- participant’s silhouette becomes material for visuals
- environment becomes denser / deeper / more spatial
- system becomes less directly reactive

### Reset

If no meaningful participant input is detected for approximately **8–15 seconds**:

**engagement decreases → visuals dissolve → system returns to dormant**

Exact timing should be tested.

---

# 9. Simplified Sensing Variables

For this first prototype, only build three core sensing values.

## Presence

`0 → 1`

Is a person currently within the interaction zone?

## Movement

`0 → 1`

How much is the participant changing between frames?

Possible method:

- frame difference
- optical flow
- silhouette change

## Engagement

`0 → 1`

How long has the participant remained present and interacting?

This becomes the most important control variable.

Conceptually:

```text
sustained presence
        ↓
engagement rises
        ↓
system deepens
```

When presence disappears:

```text
engagement gradually decays
        ↓
system dissolves
```

---

## 10. Development Parameters to Expose in TD

Keep these values visible while prototyping:

```text
presence
motion
engagement
idle_time
ai_trigger
system_state
```

This will make later tuning much easier.

---

# 11. First Visual Pipeline

Use the webcam as material rather than showing the webcam directly.

Possible TD pipeline:

```text
Webcam
↓
Body / silhouette isolation
↓
Blur / threshold
↓
Feedback
↓
Displacement
↓
Edge extraction
↓
Noise deformation
↓
Trails / persistence
↓
Compositing
↓
Large abstract visual field
```

### As engagement rises:

- displacement increases
- feedback persistence increases
- visual depth increases
- texture density increases
- the silhouette becomes less literal
- contrast / brightness can gradually intensify
- motion may become slower and more monumental
- participant remains intermittently recognizable

---

# 12. AI Integration

AI should not be the first thing built this week.

First prove:

> Presence → Engagement → Transformation

using native TouchDesigner.

### Stretch Goal

When engagement crosses a threshold such as:

```text
engagement > 0.65
```

then:

```text
capture participant frame
↓
send image to AI pipeline
↓
generate transformed image
↓
return image to TouchDesigner
↓
blend gradually into real-time visual field
```

Important shift:

### Avoid

```text
specific gesture
→ AI generation
```

### Prefer

```text
sustained attention
→ system deepens
→ AI manifestation becomes possible
```

This makes the AI feel earned within the ritual.

---

# 13. Semester Timeline

Semester target: **mid-December 2026**

Approximately ten weeks remain from October 5.

## Week of Oct 5
### Attunement Prototype

Build:

- presence detection
- movement measurement
- engagement variable
- wake / sleep logic
- first visual response

## Week of Oct 12
### Body as Visual Material

Explore:

- silhouette
- feedback
- deformation
- abstraction
- visual reciprocity

## Week of Oct 19
### AI Manifestation

Build:

- participant frame capture
- AI image transformation
- return generated image into TD
- blend with live system

## Week of Oct 26
### Continuous Ritual Logic

Integrate:

- engagement
- AI
- body transformation
- real-time visual system

Focus on continuity rather than discrete effects.

## Week of Nov 2
### Peak + Dissolution

Design:

- escalation
- climax
- inactivity detection
- closure
- return to dormant

## Week of Nov 9
### Visual Language

Test 2–3 aesthetic directions.

Choose one final visual system.

Avoid continuing multiple visual styles after this point.

## Week of Nov 16
### Sound + Spatial Experience

Develop:

- sound behavior
- scale
- screen composition
- viewing distance
- darkness
- installation conditions

### Target

Have the **core feature set complete by approximately Nov 16–23**.

## Week of Nov 23
### User Testing

Test with real participants.

Observe:

- how quickly people realize it is interactive
- whether they remain with the piece
- whether they experiment without instruction
- whether interaction feels like control or reciprocity
- whether the escalation feels meaningful
- whether the ending feels intentional

## Week of Nov 30
### Integration + Reliability

Focus on:

- stability
- timing
- transitions
- performance
- removing unnecessary features
- installation reliability

## Dec 7–15
### Final Installation + Documentation

Focus on:

- polish
- installation
- presentation
- screen recordings
- final diagrams
- documentation
- project photography / video
- critique preparation

Avoid adding major new systems during this period.

---

# 14. Three Major Milestones

## Milestone 01 — The Shrine Responds
**October 5–25**

The participant can:

- approach
- wake the system
- influence it
- deepen the interaction
- walk away
- cause it to dissolve

AI does not need to be visually resolved yet.

---

## Milestone 02 — The Shrine Transforms You
**October 26–November 16**

The participant’s body becomes generative material.

AI enters the system.

The experience reaches a convincing audiovisual manifestation / peak.

---

## Milestone 03 — The Shrine Feels Like a Place
**November 17–December 15**

Stop prioritizing new features.

Focus on:

- scale
- sound
- darkness
- silence
- pacing
- latency
- viewing distance
- spatial positioning
- transition speed
- installation
- documentation

The final question is not whether the computer vision works.

The final question is:

> Does a different spatial and psychological condition emerge?

---

# 15. Visual Reference Library

Create a project folder:

```text
Digital Shrine — Visual References/

01_ATMOSPHERE
02_VISUAL_LANGUAGE
03_BODY + FIGURE
04_SCREEN + INSTALLATION
05_MOTION
06_COLOR + LIGHT
07_AI_REFERENCES
08_REJECTED / ANTI-REFERENCES

STYLE_BIBLE.md
```

---

## 16. Reference Categories

### 01_ATMOSPHERE

Examples:

- sacred spaces
- darkness
- haze
- light shafts
- architectural scale
- ritual atmosphere

### 02_VISUAL_LANGUAGE

Examples:

- abstract sacred imagery
- generative systems
- architectural ornament
- vegetation
- particles
- fields
- textures

### 03_BODY + FIGURE

Examples:

- silhouettes
- distorted bodies
- apparition
- gesture
- human-to-image transformation

### 04_SCREEN + INSTALLATION

Examples:

- large media walls
- projection
- immersive screens
- viewing distance
- spatial composition

### 05_MOTION

Examples:

- slow movement
- feedback
- accumulation
- dissolution
- ritual pacing

### 06_COLOR + LIGHT

Examples:

- palettes
- black levels
- brightness
- contrast
- glow
- controlled highlights

### 07_AI_REFERENCES

Examples:

- AI image styles
- desired abstraction level
- image-to-image transformations
- visual coherence
- architectural / organic hybrids

### 08_REJECTED / ANTI-REFERENCES

Examples of aesthetics the project should intentionally avoid.

This is especially useful for preventing the project from becoming:

- generic AI mysticism
- cyberpunk
- glowing-particle screensaver
- overly literal religious imagery
- decorative rather than experiential

---

# 17. Reference Confidence Levels

When adding images, classify them as:

## CORE

> I want the project to feel like this.

## ELEMENT

> I only want a particular quality from this reference.

Examples:

- lighting
- movement
- composition
- texture
- scale

## ANTI-REFERENCE

> This is close to the topic, but I explicitly do not want this aesthetic.

---

# 18. STYLE_BIBLE.md

The reference folder should eventually be summarized into a short visual rule set.

Example:

```text
LIGHTING
Predominantly dark field.
Concentrated luminous areas.
Avoid colorful cyberpunk glow.

MOTION
Slow, viscous, delayed.
Avoid constant decorative movement.

BODY
Recognizable intermittently.
Avoid raw webcam aesthetics.

SACREDNESS
Produced through scale, symmetry, silence, repetition, light, and attention.
Avoid depending on obvious crosses, halos, or religious symbols.

AI IMAGERY
Architectural / organic hybrids.
High structural coherence.
Restrained visual palette.

COMPOSITION
Prefer one monumental focal field.
Avoid UI-like collections of many floating elements.
```

The Style Bible should become the shared reference used when:

- building TouchDesigner visuals
- generating AI images
- designing the final installation
- producing presentation slides
- evaluating whether new visual experiments belong in the project

---

# 19. Main Action Items

## Immediate — This Week

- [ ] Build Presence → Attunement → Deepening prototype in TouchDesigner
- [ ] Implement `presence`
- [ ] Implement `motion`
- [ ] Implement continuous `engagement`
- [ ] Implement `idle_time`
- [ ] Build dormant / wake / decay logic
- [ ] Create first abstract webcam-body visual transformation
- [ ] Test whether early feedback clearly communicates responsiveness
- [ ] Test whether later feedback can become less literal
- [ ] Decide approximate inactivity duration for dissolution
- [ ] Only after the above works, test AI image capture as a stretch goal

## Documentation

- [ ] Keep screenshots / recordings of each prototype
- [ ] Note what conceptual question each test answers
- [ ] Document failed approaches as well as successful ones
- [ ] Update the system map when interaction logic materially changes

## Visual Direction

- [ ] Collect approximately 10–20 starting visual references
- [ ] Sort into reference folders
- [ ] Tag each as CORE / ELEMENT / ANTI-REFERENCE
- [ ] Create first version of `STYLE_BIBLE.md`
- [ ] Use the Style Bible when evaluating future TD visuals

---

# 20. Working Rule Going Forward

> Every technical feature must visibly improve the ritual experience.

If a feature is technically impressive but does not strengthen:

- attention
- reciprocity
- atmosphere
- transformation
- spatial presence
- ritual pacing

then it should be simplified, postponed, or removed.
