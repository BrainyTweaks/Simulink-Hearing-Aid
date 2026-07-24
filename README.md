# Simulink Hearing Aid

> A modular digital hearing aid simulation built using **MATLAB R2025b** and **Simulink** as part of the **MathWorks Excellence in Innovation Challenge (Project 241)**.

---

## Overview

**Simulink Hearing Aid** is an engineering-focused project that models the complete Digital Signal Processing (DSP) pipeline of a modern hearing aid using MATLAB and Simulink.

Rather than simply amplifying incoming sound, modern hearing aids employ multiple DSP algorithms to improve speech intelligibility, reduce environmental noise, prevent acoustic feedback, and adapt to changing acoustic environments in real time.

This repository follows a modular architecture where each signal processing stage is developed, tested, and validated independently before being integrated into the complete hearing aid system.

---

## Objectives

- Develop a modular hearing aid simulation using MATLAB and Simulink.
- Study the DSP techniques employed in modern hearing aids.
- Design reusable and maintainable signal processing modules.
- Validate each processing stage independently before full-system integration.
- Follow professional software engineering and documentation practices.
- Maintain a reproducible project with clean repository organization.

---

## Signal Processing Pipeline

```text
Microphone
      │
      ▼
Analog-to-Digital Conversion (ADC)
      │
      ▼
Digital Filtering
      │
      ▼
Noise Reduction
      │
      ▼
Dynamic Range Compression (DRC)
      │
      ▼
Automatic Gain Control (AGC)
      │
      ▼
Adaptive Feedback Cancellation (AFC)
      │
      ▼
Equalization
      │
      ▼
Digital-to-Analog Conversion (DAC)
      │
      ▼
Speaker
```

---

## Planned Features

- Modular DSP architecture
- Digital filtering
- Multi-band equalization
- Dynamic Range Compression (DRC)
- Automatic Gain Control (AGC)
- Noise reduction
- Adaptive Feedback Cancellation (AFC)
- Real-time audio processing
- Signal visualization and analysis
- Performance evaluation
- Simulink-based system integration

---

## Repository Structure

```text
Simulink-Hearing-Aid/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── config/
│
├── src/
│
├── models/
│
├── data/
│   ├── sample/
│   └── datasets/
│
├── docs/
│   ├── assets/
│   ├── Literature Review.md
│   ├── Simulink Design Specification.md
│   ├── System Architecture.md
│   └── System Requirements Specification.md
│
├── tests/
│
└── output/
```

---

## Development Status

**Current Phase:** 🚧 Implementation

### Completed

- Repository initialization
- Project architecture
- DSP theory and research
- System design
- Documentation planning
- Repository organization

### In Progress

- MATLAB project setup
- Simulink model development
- DSP module implementation
- Independent module testing

### Upcoming

- End-to-end system integration
- Real-time performance evaluation
- System optimization
- Final validation
- Challenge submission

---

## Development Roadmap

### Phase 1 — Research & System Design

- Literature review
- Hearing aid architecture
- DSP fundamentals
- System planning

### Phase 2 — Core DSP Development

- Digital filtering
- Equalization
- Dynamic Range Compression
- Automatic Gain Control

### Phase 3 — Advanced DSP

- Noise Reduction
- Adaptive Feedback Cancellation
- Performance analysis
- Module validation

### Phase 4 — System Integration

- Simulink integration
- End-to-end testing
- Real-time evaluation
- Performance optimization

### Phase 5 — Finalization

- Documentation
- Final validation
- Repository review
- MathWorks challenge submission

---

## Software Requirements

- MATLAB R2025b
- Simulink R2025b
- Audio Toolbox
- Signal Processing Toolbox

Additional MathWorks toolboxes may be incorporated as the project evolves.

---

## Project Philosophy

This project emphasizes engineering understanding over implementation speed.

Each DSP block is developed as an independent module with a clearly defined purpose before integration into the complete hearing aid pipeline. Every design decision is documented to ensure transparency, reproducibility, and maintainability.

---

## Documentation

Project documentation is maintained within the `docs/` directory.

Current documentation includes:

- Literature Review
- System Requirements Specification
- System Architecture
- Simulink Design Specification

Additional assets, diagrams, and figures are stored in the `docs/assets/` directory.

---

## Running the Project

The implementation is currently under active development.

Once the first stable release is available, a single entry point will be provided to execute the complete hearing aid simulation with minimal setup.

---

## Challenge Information

**MathWorks Excellence in Innovation Challenge**

- **Project Number:** 241
- **Project Title:** Simulink Hearing Aid

---

## Author

**Mohammed Sharif**

---

## License

This project is licensed under the **MIT License**.