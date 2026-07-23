# Literature Review

> **Project:** Simulink Hearing Aid  
> **MathWorks Excellence in Innovation Challenge – Project #241**  
> **Author:** Mohammed Sharif  
> **Version:** 1.0

---

# 1. Introduction

Hearing loss is one of the most common sensory impairments worldwide and affects communication, education, employment, and quality of life. According to the World Health Organization (WHO), more than 1.5 billion people experience some degree of hearing loss, with hundreds of millions requiring rehabilitation through hearing devices.

Modern hearing aids are no longer simple sound amplifiers. They are sophisticated embedded Digital Signal Processing (DSP) systems capable of analysing incoming sound, enhancing speech, suppressing noise, adapting to changing environments, and compensating for individual hearing impairment.

Recent advances in digital electronics, low-power embedded processors, and signal processing algorithms have significantly improved hearing-aid performance while reducing device size and power consumption. MATLAB and Simulink provide an effective environment for modelling, simulating, analysing, and validating these algorithms before deployment to embedded hardware.

This literature review investigates the operating principles of digital hearing aids and the signal processing techniques commonly employed in commercial systems. The knowledge obtained serves as the theoretical foundation for the hearing-aid simulation developed in this project.

---

# 2. Human Hearing

## 2.1 Overview of Human Hearing

Human hearing begins when sound waves propagate through air and enter the outer ear. These pressure variations travel through the ear canal and strike the tympanic membrane (eardrum), causing it to vibrate.

The vibrations are mechanically amplified by the three ossicles (malleus, incus, and stapes) located within the middle ear. These amplified vibrations are transmitted into the cochlea, a fluid-filled spiral structure within the inner ear.

Inside the cochlea, thousands of specialised hair cells convert mechanical vibrations into electrical nerve impulses. These impulses travel through the auditory nerve to the brain, where they are interpreted as meaningful sound.

Human hearing generally spans frequencies from approximately **20 Hz to 20 kHz**, although speech information is primarily concentrated between **250 Hz and 8 kHz**.

---

## 2.2 Hearing Loss

Hearing loss occurs when one or more stages of the hearing process become impaired.

The three primary categories of hearing loss are:

### Conductive Hearing Loss

Caused by abnormalities in the outer or middle ear that reduce sound transmission.

Typical causes include:

- Earwax blockage
- Ear infections
- Damage to the eardrum
- Ossicle abnormalities

---

### Sensorineural Hearing Loss

Caused by damage to the cochlea or auditory nerve.

Common causes include:

- Age-related hearing loss (Presbycusis)
- Noise-induced hearing damage
- Genetic disorders
- Ototoxic medications

Sensorineural hearing loss is the most common form of permanent hearing impairment and is the primary condition addressed by modern digital hearing aids.

---

### Mixed Hearing Loss

A combination of conductive and sensorineural hearing loss.

---

The severity and frequency dependence of hearing loss varies considerably between individuals. Consequently, modern hearing aids employ frequency-selective processing, adaptive amplification, and dynamic range compression to personalise sound according to each user's audiogram.

---

# 3. Hearing Aids

## 3.1 Evolution of Hearing Aids

The primary purpose of a hearing aid is to improve speech intelligibility by compensating for hearing impairment while maintaining a comfortable listening experience.

Early hearing aids were purely mechanical devices, such as ear trumpets, which collected and directed sound toward the ear canal. Although simple, these devices provided only passive amplification and could not adapt to different listening environments.

The introduction of electronic hearing aids in the twentieth century enabled electrical amplification using microphones, amplifiers, and miniature loudspeakers. These analogue devices improved sound levels but amplified both speech and background noise equally, often resulting in poor listening quality.

The development of Digital Signal Processing (DSP) revolutionised hearing-aid technology. Modern digital hearing aids convert acoustic signals into digital samples using an Analog-to-Digital Converter (ADC), process the audio through multiple DSP algorithms, and reconstruct the enhanced signal using a Digital-to-Analog Converter (DAC).

Unlike analogue systems, digital hearing aids can selectively enhance speech, suppress unwanted noise, reduce acoustic feedback, and adapt automatically to changing acoustic environments.

---

## 3.2 Types of Hearing Aids

Commercial hearing aids are available in several physical configurations depending on the user's hearing loss, comfort requirements, and cosmetic preferences.

### Behind-the-Ear (BTE)

The electronic components are housed behind the ear while sound is delivered into the ear canal through a tube or receiver.

Advantages:

- Suitable for mild to profound hearing loss
- Larger battery capacity
- Easier maintenance
- Supports advanced DSP features

---

### Receiver-in-Canal (RIC)

A variation of the BTE design in which the receiver is placed inside the ear canal while the processor remains behind the ear.

Advantages:

- Improved sound quality
- Reduced acoustic feedback
- Smaller physical size

---

### In-the-Ear (ITE)

All components are integrated into a custom shell that fits inside the outer ear.

Advantages:

- Compact
- Comfortable
- Easier insertion

Limitations:

- Smaller battery
- Limited processing capability due to space constraints

---

### Completely-in-Canal (CIC)

The entire device fits deep inside the ear canal.

Advantages:

- Nearly invisible
- Natural microphone placement

Limitations:

- Very limited battery capacity
- Fewer microphones
- Reduced computational capability

---

Although their physical construction differs, the internal DSP architecture of modern hearing aids is fundamentally similar.

---

## 3.3 Digital Hearing Aids

Modern hearing aids are embedded real-time signal processing systems.

Their operation follows a continuous processing pipeline:

1. Acoustic signal acquisition using one or more microphones.
2. Analog signal conditioning.
3. Analog-to-Digital Conversion (ADC).
4. Digital Signal Processing.
5. Digital-to-Analog Conversion (DAC).
6. Audio reproduction through the receiver.

Unlike conventional audio amplifiers, digital hearing aids process different frequency bands independently. This enables personalised amplification based on the user's audiogram while preserving speech quality.

Most commercial devices include algorithms such as:

- Multi-band filtering
- Dynamic Range Compression (DRC)
- Automatic Gain Control (AGC)
- Noise Reduction (NR)
- Adaptive Feedback Cancellation (AFC)
- Beamforming
- Environment classification
- Wind-noise reduction
- Directional microphone processing

These algorithms operate continuously in real time while maintaining extremely low latency and low power consumption.

The hearing-aid simulation developed in this project focuses on recreating the core DSP processing chain used in modern digital hearing aids using MATLAB and Simulink.

---

# 4. Signal Processing in Hearing Aids

Modern hearing aids are fundamentally Digital Signal Processing (DSP) systems. Unlike conventional amplifiers, they do not simply increase the loudness of incoming sound. Instead, they analyse the acoustic environment, process the signal using multiple DSP algorithms, and reconstruct an output tailored to the user's hearing profile.

A modern hearing aid typically contains several processing stages operating sequentially in real time. Each stage addresses a specific problem encountered during everyday listening.

---

## 4.1 Digital Filtering

Filtering is one of the first operations performed inside a hearing aid.

The incoming audio contains useful speech components together with unwanted frequency components such as low-frequency hum, high-frequency electrical noise, and environmental interference.

Digital filters selectively attenuate or preserve specific frequency ranges while maintaining the overall quality of speech.

Modern hearing aids employ both Finite Impulse Response (FIR) and Infinite Impulse Response (IIR) filters depending on computational requirements and latency constraints.

Filtering also forms the basis of multi-band processing, where the audio signal is divided into several frequency bands that can be processed independently.

---

## 4.2 Dynamic Range Compression

One of the most important features of a digital hearing aid is Dynamic Range Compression (DRC).

Individuals with sensorineural hearing loss often experience reduced dynamic range, meaning soft sounds become inaudible while loud sounds quickly become uncomfortable.

Dynamic Range Compression automatically applies different amplification levels depending on the instantaneous signal amplitude.

Soft sounds receive greater amplification, whereas loud sounds receive comparatively less amplification.

Unlike simple amplification, compression preserves listening comfort while improving speech intelligibility.

Modern hearing aids typically perform compression independently within multiple frequency bands to match the user's audiogram.

---

## 4.3 Noise Reduction

Background noise is one of the primary challenges faced by hearing-aid users.

Conventional amplification increases both speech and noise simultaneously, often reducing speech intelligibility.

Noise Reduction (NR) algorithms attempt to estimate the background noise and attenuate it while preserving speech components.

Common approaches include:

- Spectral subtraction
- Wiener filtering
- Minimum Mean Square Error (MMSE) estimation
- Statistical model-based estimators

Modern hearing aids frequently combine multiple noise reduction techniques depending on the acoustic environment.

---

## 4.4 Adaptive Feedback Cancellation

One of the most common problems encountered in hearing aids is **acoustic feedback**, often perceived as a high-pitched whistling sound.

Feedback occurs when sound produced by the hearing-aid receiver leaks back into the microphone, creating a closed-loop amplification path. If left uncompensated, this positive feedback can quickly become unstable and severely degrade listening quality.

Modern digital hearing aids employ **Adaptive Feedback Cancellation (AFC)** techniques to continuously estimate the acoustic feedback path and subtract the estimated feedback signal from the microphone input.

Adaptive algorithms such as the **Least Mean Squares (LMS)** and **Normalized Least Mean Squares (NLMS)** algorithms are widely used because they can track changes in the acoustic feedback path caused by user movement, changes in ear coupling, or environmental conditions.

Adaptive feedback cancellation has become a standard component of commercial digital hearing aids due to its effectiveness, computational efficiency, and ability to improve maximum stable gain.

---

## 4.5 Multi-band Processing

Human hearing loss rarely affects all frequencies equally.

Many individuals experience greater hearing impairment at high frequencies while retaining relatively normal hearing at lower frequencies. Consequently, applying identical amplification across the entire audio spectrum often produces unsatisfactory results.

To address this limitation, modern hearing aids divide the incoming audio into multiple frequency bands using digital filter banks.

Each band is processed independently, allowing parameters such as gain, compression ratio, and noise reduction strength to be adjusted according to the user's audiogram.

Multi-band processing enables personalised hearing correction and forms the foundation of most contemporary hearing-aid signal processing architectures.

---

## 4.6 Beamforming

Many modern hearing aids incorporate multiple microphones to improve speech intelligibility in noisy environments.

Beamforming is a spatial signal processing technique that combines signals from multiple microphones to enhance sounds arriving from a desired direction while attenuating sounds arriving from other directions.

By exploiting differences in arrival time and signal phase between microphones, beamforming increases the Signal-to-Noise Ratio (SNR) for speech originating in front of the listener.

Beamforming has become an essential component of premium hearing aids, particularly in environments containing multiple competing sound sources such as restaurants, classrooms, and public spaces.

---

## 4.7 Environment Classification

Listening environments constantly change throughout the day.

For example, the signal characteristics encountered during a quiet conversation differ significantly from those experienced in moving traffic or during music playback.

Modern hearing aids employ environment classification algorithms that analyse incoming audio features such as energy, spectral distribution, modulation characteristics, and temporal statistics to determine the current acoustic environment.

Once classified, the hearing aid automatically adjusts DSP parameters including:

- Compression settings
- Noise reduction strength
- Directional microphone behaviour
- Feedback cancellation parameters

This adaptive behaviour significantly improves user comfort while reducing the need for manual adjustment.

---

# 5. MATLAB and Simulink for Hearing Aid Development

MATLAB and Simulink are widely adopted in academia and industry for the design, simulation, verification, and deployment of Digital Signal Processing (DSP) systems. Their extensive collection of specialised toolboxes enables engineers to rapidly prototype signal processing algorithms before implementing them on embedded hardware.

For hearing-aid development, MATLAB provides high-level functions for audio analysis, filter design, adaptive filtering, spectral processing, and real-time audio streaming through the Audio Toolbox and DSP System Toolbox.

Simulink complements MATLAB by providing a graphical, model-based design environment in which complex DSP systems can be represented as interconnected functional blocks. This visual representation simplifies algorithm development, verification, debugging, and system integration while closely reflecting the architecture of embedded DSP systems.

One significant advantage of the MATLAB–Simulink workflow is the ability to validate algorithms using prerecorded datasets or live audio before hardware deployment. Once validated, models can be further extended for embedded implementation through automatic code generation workflows supported by MathWorks.

For educational and research-oriented projects, this integrated environment significantly reduces development time while improving system readability, modularity, and reproducibility.

For these reasons, MATLAB and Simulink have been selected as the primary development environment for this hearing-aid simulation.

---

# 6. Research Gap

Commercial digital hearing aids employ highly sophisticated proprietary algorithms that are generally unavailable for public study due to intellectual property restrictions.

Although numerous publications describe individual algorithms such as adaptive feedback cancellation, dynamic range compression, or noise reduction, complete end-to-end hearing-aid implementations are rarely available as open-source educational resources.

Existing examples often focus on isolated DSP techniques without demonstrating how these algorithms interact within a complete real-time processing pipeline.

Furthermore, many educational implementations emphasise theoretical derivations while providing limited opportunities for system-level modelling and simulation.

This project attempts to bridge this gap by developing a modular hearing-aid simulation that combines the major DSP building blocks commonly found in commercial devices within a single Simulink model. The resulting system provides an educational platform for understanding both individual algorithms and their interaction within an integrated real-time signal processing architecture.

---

# 7. Design Motivation

The design of this project is motivated by three primary objectives.

First, to understand the architecture and operation of modern digital hearing aids from an engineering perspective rather than treating them as black-box devices.

Second, to apply fundamental Digital Signal Processing concepts, including filtering, adaptive filtering, dynamic range compression, and spectral processing, to a practical biomedical engineering application.

Finally, to develop a modular and extensible MATLAB–Simulink implementation that closely resembles the signal-processing pipeline employed in commercial hearing-aid systems while remaining sufficiently simple for educational exploration and experimentation.

---

# References

[1] World Health Organization (WHO), *World Report on Hearing*, Geneva, Switzerland, 2021. ([View Report](https://www.who.int/publications/i/item/world-report-on-hearing))

[2] S. Launer, J. A. Zakis, and B. C. J. Moore, *Hearing Aid Signal Processing*, Springer, 2016.

[3] H. Puder, "Hearing Aids: An Overview of the State-of-the-Art, Challenges, and Future Trends of an Interesting Audio Signal Processing Application," *ISPA*, 2009.

[4] J. M. Kates, "Principles of Digital Dynamic-Range Compression," *Trends in Amplification*, vol. 9, no. 2, pp. 45–76, 2005.

[5] F. Strasser and H. Puder, "Adaptive Feedback Cancellation for Realistic Hearing Aid Applications," *IEEE/ACM Transactions on Audio, Speech, and Language Processing*, vol. 23, no. 12, pp. 2322–2333, 2015.

[6] MathWorks, *MATLAB and Simulink for Hearing Aids*. ([Documentation](https://www.mathworks.com/solutions/medical-devices/hearing-aids.html))

[7] MathWorks, *Audio Toolbox Documentation*. ([Documentation](https://www.mathworks.com/products/audio.html))

[8] MathWorks, *DSP System Toolbox Documentation*. ([Documentation](https://www.mathworks.com/products/dsp-system.html))

[9] MathWorks, *Real-Time Audio in Simulink*. ([Documentation](https://www.mathworks.com/help/audio/gs/real-time-audio-in-simulink.html))

[10] MathWorks, *Real-Time Audio in MATLAB*. ([Documentation](https://www.mathworks.com/help/audio/gs/real-time-audio-in-matlab.html))

[11] MathWorks, *Audio Processing Algorithm Design*. ([Documentation](https://www.mathworks.com/help/audio/audio-processing-algorithm-design.html))

[12] MathWorks, *Acoustic Noise Cancellation Using LMS*. ([Example](https://www.mathworks.com/help/audio/ug/acoustic-noise-cancellation-using-lms.html))

[13] MathWorks, *Cochlear Implant Speech Processor*. ([Example](https://www.mathworks.com/help/audio/ug/cochlear-implant-speech-processor.html))

[14] MathWorks, *Active Noise Control with Simulink*. ([Documentation](https://www.mathworks.com/help/audio/ug/active-noise-control-with-simulink.html))

[15] MathWorks Excellence in Innovation Challenge, Project #241, *Simulink Hearing Aid*. ([Project Page](https://github.com/mathworks/MATLAB-Simulink-Challenge-Project-Hub))