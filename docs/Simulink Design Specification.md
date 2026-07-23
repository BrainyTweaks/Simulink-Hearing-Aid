# Simulink Design Specification

> **Project:** Simulink Hearing Aid  
> **MathWorks Excellence in Innovation Challenge – Project #241**  
> **Author:** Mohammed Sharif  
> **Version:** 1.0

---

# Purpose

This document defines the complete Simulink implementation strategy for the Hearing Aid simulation.

Unlike the System Architecture document, which explains the overall organisation of the project, this document specifies how each subsystem will be realised inside Simulink.

Every subsystem described here corresponds directly to one Simulink subsystem that will later be implemented, tested, and verified.

The primary objectives are:

- Define subsystem boundaries.
- Identify required Simulink blocks.
- Specify subsystem inputs and outputs.
- Define testing methodology.
- Maintain a modular and scalable model.

---

# Simulink Model Structure

The complete Simulink model shall consist of the following independent subsystems.

```
Simulink Hearing Aid

├── Audio Source
├── Pre-processing
├── Frequency Filtering
├── Dynamic Range Compression
├── Noise Reduction
├── Adaptive Feedback Cancellation
├── Output Gain
└── Audio Output
```

Each subsystem shall perform a single dedicated processing task.

Communication between subsystems shall occur through sequential signal flow only.

No subsystem shall directly modify the internal operation of another subsystem.

This design improves:

- readability,
- maintainability,
- debugging,
- scalability,
- testing.

---

# Development Strategy

The complete system shall be developed incrementally.

Only one subsystem will be implemented and validated at a time.

The development order is:

1. Audio Source
2. Pre-processing
3. Frequency Filtering
4. Dynamic Range Compression
5. Noise Reduction
6. Adaptive Feedback Cancellation
7. Output Gain
8. Complete System Integration

Each subsystem shall satisfy its individual test cases before integration into the complete system.

No subsystem shall be added until the previous subsystem has been verified.

---

# Modeling Principles

The Simulink implementation shall follow the following engineering principles.

- Modular subsystem design.
- Single responsibility per subsystem.
- Frame-based audio processing.
- Relative signal routing.
- Minimal manual parameter changes.
- Clearly named blocks and signals.
- Consistent subsystem hierarchy.
- Support future expansion without redesign.

These principles ensure that the model remains understandable and reusable throughout future development.

---

# Subsystem 1 — Audio Source

## Purpose

The Audio Source subsystem provides the input signal to the hearing-aid processing pipeline.

It is responsible for acquiring audio from either a real-time microphone or a prerecorded audio file and supplying a continuous digital audio stream to the remaining system.

No signal processing shall be performed inside this subsystem.

Its only responsibility is reliable audio acquisition.

---

## Inputs

None.

The subsystem directly interfaces with an external audio source.

Possible sources include:

- Live microphone
- WAV audio file
- Test signal (optional during debugging)

---

## Outputs

The subsystem outputs a single-channel digital audio stream.

Output signal:

```
Audio Frame
```

Data type:

```
double
```

Signal format:

```
Frame-based
```

---

## Simulink Blocks Required

Primary block:

- Audio Device Reader

Alternative testing block:

- From Multimedia File

Optional debugging blocks:

- Sine Wave
- Signal Generator

---

## MATLAB Toolbox Required

- Audio Toolbox

---

## Configuration Parameters

The following parameters will initially be used.

| Parameter | Initial Value |
|-----------|--------------:|
| Sample Rate | 16 kHz |
| Channels | 1 (Mono) |
| Samples Per Frame | 256 |
| Data Type | double |

These values may be adjusted later during system optimisation.

---

## Processing

This subsystem performs no DSP operations.

Its responsibilities are limited to:

- acquiring audio,
- buffering samples into frames,
- forwarding frames to the Pre-processing subsystem.

---

## Expected Behaviour

The subsystem shall:

- continuously acquire audio,
- produce one frame at a time,
- operate without interruption,
- maintain a constant sample rate,
- introduce negligible processing delay.

---

## Verification

The subsystem is considered correct if:

- live microphone audio is successfully acquired,
- prerecorded audio can be loaded,
- output frames are continuous,
- no dropped frames occur,
- the Audio Time Scope displays the expected waveform.

---

## Future Improvements

Possible future extensions include:

- Automatic source selection
- Stereo audio support
- Bluetooth microphone input
- Multi-microphone array input
- Raspberry Pi microphone interface

---

# Subsystem 2 — Pre-processing

## Purpose

The Pre-processing subsystem prepares the incoming audio signal for reliable Digital Signal Processing (DSP).

Raw microphone signals often contain small imperfections such as DC offset, inconsistent amplitude levels, and framing issues. These characteristics can negatively affect subsequent DSP algorithms if not corrected.

This subsystem ensures that all downstream processing stages receive a stable, well-conditioned digital signal.

---

## Inputs

Input signal:

```
Audio Frame
```

Source:

- Audio Source subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

```
Pre-processed Audio Frame
```

The output maintains the same sampling frequency and frame size as the input while providing a conditioned signal suitable for DSP operations.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- DC Blocker (or High-Pass Filter)
- Gain
- Buffer (if required)
- Data Type Conversion (if required)

The exact block selection may change during implementation depending on Audio Toolbox availability.

---

## MATLAB Toolbox Required

- DSP System Toolbox
- Audio Toolbox

---

## Configuration Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Sample Rate | 16 kHz |
| Samples Per Frame | 256 |
| Channels | 1 |
| Data Type | double |

---

## Processing Operations

The subsystem performs the following operations.

### 1. DC Offset Removal

Any constant offset present in the microphone signal shall be removed.

Removing the DC component prevents unnecessary bias during filtering and dynamic range compression.

---

### 2. Signal Normalisation

The incoming audio amplitude shall be adjusted to a consistent operating range.

Normalisation prevents excessive amplification or attenuation in later processing stages.

---

### 3. Frame Preparation

Incoming audio shall remain organised as fixed-size frames.

Maintaining consistent frame sizes simplifies buffering and enables efficient real-time DSP algorithms.

---

### 4. Data Type Verification

The subsystem shall ensure that the audio remains in the expected numeric format before entering the filtering stage.

---

## Expected Behaviour

The subsystem shall:

- remove DC offset,
- maintain continuous frame timing,
- preserve speech information,
- avoid introducing audible distortion,
- produce a stable signal for filtering.

---

## Verification

The subsystem is considered correct if:

- the waveform is centred around zero,
- no clipping occurs,
- frame continuity is preserved,
- output amplitude remains within the expected operating range,
- the Audio Time Scope displays a stable waveform.

---

## Future Improvements

Possible future extensions include:

- Automatic Gain Control (AGC)
- Voice Activity Detection (VAD)
- Input Level Monitoring
- Automatic Microphone Calibration

---

# Subsystem 3 — Frequency Filtering

## Purpose

The Frequency Filtering subsystem removes unwanted frequency components from the incoming audio signal while preserving the frequency range most important for speech perception.

This subsystem represents the first Digital Signal Processing (DSP) stage of the hearing-aid pipeline.

---

## Inputs

Input signal:

```
Pre-processed Audio Frame
```

Source:

- Pre-processing subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

```
Filtered Audio Frame
```

The output contains reduced low-frequency and high-frequency noise while preserving speech information.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- Digital Filter
- FIR Filter
- Filter Designer (optional during design)

---

## MATLAB Toolbox Required

- DSP System Toolbox
- Signal Processing Toolbox

---

## Filter Type

The proposed hearing aid shall initially employ a **Finite Impulse Response (FIR)** digital filter.

Reasons for selecting FIR include:

- Guaranteed stability
- Linear phase response
- Minimal waveform distortion
- Simple implementation within Simulink
- Excellent suitability for educational DSP applications

Future versions may investigate IIR filtering to reduce computational complexity.

---

## Processing Operations

The subsystem performs the following tasks.

### 1. Low-Frequency Noise Suppression

Reduce unwanted components such as:

- microphone rumble,
- handling noise,
- environmental vibrations.

---

### 2. High-Frequency Noise Suppression

Reduce unwanted high-frequency components including:

- electrical noise,
- microphone hiss,
- high-frequency interference.

---

### 3. Speech Preservation

Preserve the frequency range most important for speech intelligibility while suppressing frequencies that contribute little useful information.

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Sampling Frequency | 16 kHz |
| Filter Type | FIR |
| Processing Mode | Frame-Based |

The exact cutoff frequencies and filter order will be determined during implementation and experimental validation.

---

## Expected Behaviour

The subsystem shall:

- attenuate unwanted frequencies,
- preserve speech quality,
- avoid audible distortion,
- operate continuously in real time,
- introduce minimal processing delay.

---

## Verification

The subsystem is considered correct if:

- unwanted frequency components are attenuated,
- speech remains intelligible,
- frequency response matches the intended design,
- no instability occurs,
- Audio Spectrum Analyzer confirms expected filtering behaviour.

---

## Future Improvements

Possible future extensions include:

- Adaptive filtering
- Multi-band filtering
- User-selectable hearing profiles
- Frequency-dependent amplification

---

# Subsystem 4 — Dynamic Range Compression

## Purpose

The Dynamic Range Compression subsystem automatically adjusts the amplification applied to incoming audio according to its instantaneous signal level.

Individuals with hearing loss often experience reduced sensitivity to soft sounds while simultaneously finding loud sounds uncomfortable.

This subsystem compensates for that behaviour by selectively amplifying low-level signals and limiting amplification for high-level signals.

---

## Inputs

Input signal:

```
Filtered Audio Frame
```

Source:

- Frequency Filtering subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

```
Compressed Audio Frame
```

The output maintains improved speech audibility while reducing excessive loudness.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- Dynamic Range Compressor
- Gain
- Math Function
- MATLAB Function Block (if required)

---

## MATLAB Toolbox Required

- Audio Toolbox
- DSP System Toolbox

---

## Processing Principle

The subsystem continuously estimates the signal level of the incoming audio.

The applied gain is then adjusted automatically according to the signal amplitude.

The following behaviour shall be achieved:

- Quiet sounds receive greater amplification.
- Moderate sounds receive moderate amplification.
- Loud sounds receive reduced amplification.
- Extremely loud sounds shall not be amplified further.

This adaptive gain adjustment improves listening comfort while preserving speech intelligibility.

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Processing Mode | Frame-Based |
| Compression Type | Wide Dynamic Range Compression (WDRC) |
| Automatic Gain Control | Enabled |

Compression threshold, attack time, release time, compression ratio, and make-up gain shall be determined during implementation and optimisation.

---

## Expected Behaviour

The subsystem shall:

- increase audibility of weak speech,
- reduce excessive loudness,
- minimise sudden gain changes,
- preserve natural speech characteristics,
- operate continuously in real time.

---

## Verification

The subsystem is considered correct if:

- low-level speech becomes more audible,
- loud sounds remain comfortable,
- gain changes occur smoothly,
- speech remains intelligible,
- no clipping or audible pumping occurs.

---

## Future Improvements

Possible future extensions include:

- Multi-band compression
- Adaptive compression ratios
- User-specific hearing profiles
- Frequency-dependent gain adjustment
- AI-assisted loudness adaptation

---

# Subsystem 5 — Noise Reduction

## Purpose

The Noise Reduction subsystem suppresses unwanted background noise while preserving speech intelligibility.

Following dynamic range compression, both speech and background noise may have been amplified. This subsystem selectively attenuates noise components to improve listening comfort without significantly affecting the clarity of speech.

---

## Inputs

Input signal:

```
Compressed Audio Frame
```

Source:

- Dynamic Range Compression subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

```
Noise-Reduced Audio Frame
```

The output shall contain enhanced speech with reduced background noise.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- Noise Suppression
- Spectral Processing Blocks
- MATLAB Function Block (if required)

The final implementation may combine multiple DSP blocks depending on the selected algorithm.

---

## MATLAB Toolbox Required

- Audio Toolbox
- DSP System Toolbox

---

## Processing Principle

The subsystem estimates the background noise present within each audio frame and reduces its contribution while preserving speech.

The processing objective is not to eliminate all noise, but rather to improve the signal-to-noise ratio (SNR) while maintaining natural speech quality.

The subsystem shall avoid introducing artificial distortion or excessive speech suppression.

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Processing Mode | Frame-Based |
| Noise Estimation | Adaptive |
| Speech Preservation | Enabled |

Algorithm-specific parameters will be selected during implementation and validation.

---

## Expected Behaviour

The subsystem shall:

- suppress stationary background noise,
- preserve speech clarity,
- minimise speech distortion,
- avoid musical noise artefacts,
- operate continuously in real time.

---

## Verification

The subsystem is considered correct if:

- background noise is noticeably reduced,
- speech remains clearly understandable,
- speech quality is preserved,
- signal continuity is maintained,
- objective and subjective listening tests indicate improved listening comfort.

---

## Future Improvements

Possible future extensions include:

- Spectral subtraction
- Wiener filtering
- Minimum Mean Square Error (MMSE) estimation
- Deep-learning-based speech enhancement
- Environment-aware adaptive noise suppression

---

# Subsystem 6 — Adaptive Feedback Cancellation

## Purpose

The Adaptive Feedback Cancellation subsystem reduces acoustic feedback generated between the hearing-aid output speaker and input microphone.

When amplified sound from the speaker is received again by the microphone, a feedback loop is created. If the loop gain becomes sufficiently high, it produces an unstable oscillation commonly perceived as a high-pitched whistling sound.

This subsystem estimates the feedback path and generates a cancellation signal to maintain system stability.

---

## Inputs

Input signals:

```
Noise-Reduced Audio Frame
```

and

```
Estimated Feedback Signal
```

Sources:

- Noise Reduction subsystem
- Adaptive filter output

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

```
Feedback-Cancelled Audio Frame
```

The output shall contain reduced feedback components while preserving the desired audio signal.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- LMS Adaptive Filter
- FIR Filter
- Delay Block
- Sum Block
- MATLAB Function Block (if required)

---

## MATLAB Toolbox Required

- DSP System Toolbox
- Audio Toolbox

---

## Processing Principle

The subsystem uses adaptive filtering to estimate the acoustic feedback path between the speaker and microphone.

The estimated feedback signal is subtracted from the microphone signal to reduce the unwanted feedback component.

The adaptive filter continuously updates its coefficients to track changes in the acoustic environment.

The general adaptive filtering process consists of:

1. Estimate feedback path.
2. Generate predicted feedback signal.
3. Subtract estimated feedback from incoming signal.
4. Update filter coefficients.

---

## Adaptive Algorithm

The initial implementation shall investigate the Least Mean Squares (LMS) adaptive filtering algorithm.

Reasons for selecting LMS:

- Simple mathematical structure.
- Low computational complexity.
- Suitable for real-time DSP applications.
- Available in MATLAB and Simulink environments.

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Adaptive Algorithm | LMS |
| Filter Type | FIR Adaptive Filter |
| Processing Mode | Frame-Based |
| Adaptation | Continuous |

The adaptive filter order and step size will be selected during implementation and testing.

---

## Expected Behaviour

The subsystem shall:

- reduce acoustic feedback,
- maintain system stability,
- adapt to changing acoustic conditions,
- introduce minimal additional delay,
- preserve speech quality.

---

## Verification

The subsystem is considered correct if:

- feedback oscillations are reduced,
- output remains stable at higher gain levels,
- adaptive coefficients converge properly,
- desired audio is not significantly distorted.

Testing shall include controlled feedback scenarios by increasing speaker-to-microphone coupling.

---

## Future Improvements

Possible future extensions include:

- Normalised LMS (NLMS)
- Frequency-domain adaptive filtering
- Feedback path modelling improvement
- Acoustic environment adaptation
- AI-assisted feedback prediction

# Subsystem 7 — Output Gain

## Purpose

The Output Gain subsystem applies the final amplification adjustment required before audio playback.

In a real digital hearing aid, amplification is customized according to the user's hearing profile. Different individuals require different levels of gain depending on the degree and type of hearing loss.

This subsystem represents the final fitting stage where the processed audio signal is adjusted to achieve the required listening level while maintaining comfort and preventing excessive amplification.

---

## Inputs

Input signal:

    Feedback-Cancelled Audio Frame

Source:

- Adaptive Feedback Cancellation subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

    Amplified Audio Frame

The output represents the final processed audio signal before conversion into an acoustic output.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- Gain
- Variable Gain
- Lookup Table
- MATLAB Function Block (if required)

---

## MATLAB Toolbox Required

- Audio Toolbox
- DSP System Toolbox

---

## Processing Principle

The subsystem applies the final gain adjustment to compensate for reduced auditory sensitivity experienced by individuals with hearing impairment.

The applied gain shall be controlled to achieve the following objectives:

- Soft sounds become audible.
- Speech remains understandable.
- Loud sounds remain comfortable.
- Output levels remain within safe limits.

The gain value may be implemented as:

- Fixed gain during initial development.
- User-adjustable gain for simulation.
- Frequency-dependent gain for advanced hearing loss compensation.

---

## Gain Control Strategy

The initial implementation shall investigate a programmable gain model.

The gain adjustment process:

    Processed Audio Frame
              |
              ▼
        Gain Adjustment
              |
              ▼
       Amplified Audio Frame

For future improvements, the gain stage may incorporate audiogram-based fitting rules where different frequency regions receive different amplification levels based on the user's hearing profile.

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Processing Mode | Frame-Based |
| Gain Control | Adjustable |
| Gain Type | Linear Gain |
| User Profile | Generic Hearing Loss Model |

The final gain values shall be optimized during simulation and listening evaluation.

---

## Expected Behaviour

The subsystem shall:

- provide controlled amplification,
- avoid signal clipping,
- maintain speech quality,
- prevent excessive loudness,
- allow adjustment for different hearing profiles.

---

## Verification

The subsystem is considered correct if:

- output amplitude increases according to the selected gain,
- waveform characteristics are preserved,
- clipping does not occur,
- speech remains intelligible,
- output loudness improves compared to the unprocessed signal.

Testing shall include:

- low-level speech signals,
- normal speech signals,
- high-amplitude signals.

---

## Future Improvements

Possible future extensions include:

- Audiogram-based gain fitting
- Multi-band gain control
- Prescription-based hearing aid fitting
- Adaptive loudness normalization
- User-specific hearing profiles

---

## Engineering Decision

A separate output gain stage is included instead of combining gain control with earlier processing blocks.

This provides:

- modular design,
- easier parameter tuning,
- compatibility with different hearing profiles,
- similarity to commercial digital hearing aid architectures.

The separation allows future implementation of advanced fitting algorithms without modifying the main DSP pipeline.

# Subsystem 8 — Audio Output

## Purpose

The Audio Output subsystem converts the final processed digital audio signal into an audible output.

This subsystem represents the final stage of the digital hearing aid system, where the enhanced audio signal is delivered to the user through an output device such as a speaker, earphone, or hearing aid receiver.

The purpose of this subsystem is to provide the final interface between the digital signal processing system and the listener.

---

## Inputs

Input signal:

    Amplified Audio Frame

Source:

- Output Gain subsystem

Signal format:

- Frame-based
- Mono
- double precision

---

## Outputs

Output signal:

    Acoustic Audio Output

The output represents the final enhanced audio signal perceived by the listener.

---

## Simulink Blocks Required

Expected Simulink blocks include:

- Audio Device Writer
- Audio Playback
- DAC Interface (for hardware implementation)
- Scope
- Spectrum Analyzer

---

## MATLAB Toolbox Required

- Audio Toolbox
- DSP System Toolbox

---

## Processing Principle

The Audio Output subsystem performs the final conversion of the processed digital audio signal into an audible form.

During simulation, the processed audio frame is sent to the computer audio interface for playback and evaluation.

The processing flow is:

    Digital Audio Signal
              |
              ▼
     Audio Playback Interface
              |
              ▼
        Acoustic Output

For future hardware implementation, the output stage represents the conversion and driving circuitry required for a physical hearing aid device.

The hardware-oriented signal flow is:

    Digital Signal
          |
          ▼
    DAC Conversion
          |
          ▼
      Amplifier
          |
          ▼
   Speaker / Receiver

---

## Initial Design Parameters

| Parameter | Initial Value |
|-----------|--------------:|
| Output Channel | Mono |
| Sample Rate | Same as Input |
| Output Device | System Audio Device |
| Processing Mode | Real-Time Playback |

---

## Expected Behaviour

The subsystem shall:

- reproduce the enhanced audio signal,
- maintain the original sampling rate,
- minimize additional processing delay,
- provide real-time playback capability,
- preserve the improvements achieved by previous processing stages.

---

## Verification

The subsystem is considered correct if:

- processed audio can be played successfully,
- no additional distortion is introduced,
- latency remains acceptable,
- enhanced speech is clearly audible.

Verification shall include:

- waveform comparison between input and output,
- frequency spectrum analysis,
- listening evaluation,
- real-time playback testing.

---

## Future Improvements

Possible future extensions include:

- Real-time hardware deployment
- Embedded audio codec integration
- STM32/ARM-based implementation
- Bluetooth audio output
- Low-power hearing aid hardware design
- Dedicated DAC and amplifier circuit integration

---

## Engineering Decision

The Audio Output subsystem is kept independent from the DSP processing chain to maintain portability and modularity.

Separating the output stage allows the same DSP architecture to be deployed across different platforms:

- MATLAB simulation environment
- Simulink Real-Time systems
- Embedded DSP processors
- ARM-based hearing aid hardware platforms

This modular approach ensures that improvements in the DSP pipeline can be transferred easily to future hardware implementations.