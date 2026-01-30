# YM2163_SPFM_Piano_MIDI_v10
The new version of the YM2163 MIDI driver supports 4x YM2163 and has optimized the channel allocation algorithm.

**YM2163 Virtual Piano v10 — Control Software Architecture**

A high-performance, real-time control application for the RE:SPFM Quad YM2163 hardware platform. Built on the Dear ImGui immediate-mode GUI framework, the software provides comprehensive MIDI routing, intelligent voice allocation, adaptive dynamics processing, and hardware state visualization.

---

## Core Modules

### 1. MIDI Input & Routing Engine

**Input Sources**
- USB MIDI devices (class-compliant)
- Virtual MIDI cables (loopback drivers)
- File-based playback (Standard MIDI Files, Format 0/1)

**Event Processing Pipeline**
- Real-time MIDI event parsing with sub-millisecond latency
- Support for Note On/Off, Control Change (CC#64 Sustain, CC#7 Volume, CC#1 Modulation), Program Change, Pitch Bend
- Running status detection for bandwidth optimization
- Timestamp synchronization with hardware clock

**Channel Mapping**
- 16 MIDI input channels → 24 physical YM2163 channels (distributed across 4 slots)
- Omni mode support (receive on all channels)
- Monophonic/Polyphonic mode per slot

---

### 2. Dynamic Voice Allocation System

**Allocation Strategy**
- **First-fit search**: Locate inactive channel (KON=0) in sequential slot order (Slot0 → Slot3)
- **Priority-based stealing**: When all channels active, select note for termination based on:
  - Age of note (oldest first)
  - Velocity value (lowest priority)
  - Envelope state (notes in release phase preferred)

**Slot Management**
- Independent enable/disable per slot (YM2163 Chip 1-4)
- Hot-swapping capability: dynamic insertion/removal of chips without audio interruption
- Per-slot muting and solo functionality

---

### 3. Adaptive Velocity Mapping Engine

**Statistical Analysis**
Continuous histogram computation over sliding window (1024 note events):
- Peak velocity detection (95th percentile)
- Most common velocity clustering
- Dynamic range assessment of input material

**Adaptive Threshold Algorithm**
Automatically adjusts volume attenuation mapping based on input characteristics:

| Input Velocity Range | Attenuation | YM2163 Register |
|---------------------|-------------|-----------------|
| ≥ 90th percentile | 0dB | VL1=0, VL2=0 |
| 66–89 | -6dB | VL1=1, VL2=0 |
| 64–65 | -12dB | VL1=0, VL2=1 |
| < 10 | Mute | Key-on suppressed |

This ensures optimal dynamic contrast regardless of source MIDI compression or recording velocity curves.

---

### 4. Real-Time Hardware Control

**Register-Level Interface**
- Direct manipulation of all YM2163 registers (0x80–0x9F per chip)
- Atomic write operations for frequency/divider updates
- Read-back capability for status register polling

**Synchronization Modes**
- **Live Control**: Immediate register updates on MIDI input
- **Config Mode**: Buffered batch updates for parameter changes
- **Diagnostic Mode**: Register inspection and hardware testing

---

### 5. Synthesis Parameter Control

**Timbre Selection**
Per-channel waveform assignment:
- Hc (Half-Coded)
- St (Strings)
- Or (Organ)
- Cl (Clarinet)
- Pf (Piano)

**Envelope Configuration**
Four preset envelope curves:
- **Decay**: Percussive pluck (60ms decay, 1.2s release)
- **Fast**: Piano with hammer noise (60ms attack, 120ms decay)
- **Medium**: Staccato organ (instant attack, sustain hold)
- **Slow**: Sustained strings (full sustain, long release)

**Pedal Modes**
- **Disabled**: No sustain processing
- **Piano Pedal**: Standard damper behavior (SUS bit control)
- **Organ Pedal**: Legato mode with retrigger suppression

**Global Controls**
- Master volume (0dB to -∞)
- Octave transposition (±2 octaves)
- Fine tuning (±100 cents, 0.1 cent resolution)
- Auto-skip silence for MIDI playback

---

### 6. Visualization & Monitoring

**Virtual Keyboard**
- 61-key display (C4–C7 standard, expandable)
- Real-time key highlighting with velocity-based color intensity
- Sustain pedal state indication

**Per-Slot Activity Display**
- Channel usage indicators (CH0–CH3 active/inactive/release state)
- Rhythm channel VU meters (BD/HC/SDN/HHO/HHD) with peak hold
- Real-time envelope phase visualization

**System Log**
- MIDI event tracing with timestamp
- Velocity statistics (peak, histogram, threshold boundaries)
- Error reporting (buffer overflow, communication timeout)
- Auto-scroll with manual pause capability

---

### 7. File Management & Playback

**MIDI File Browser**
- Hierarchical directory navigation
- File metadata display (duration, track count, tempo)
- Playlist creation and management
- Recent folder history with quick access

**Playback Controls**
- Transport: Play/Pause/Stop/Previous/Next
- Progress bar with time display (current/total)
- Auto-play mode: Sequential or random
- Tempo override: 25%–200% of original BPM

---

### 8. Configuration Management

**Preset System**
- Save/load complete system state (timbre, envelope, pedal, velocity mapping)
- Config file import/export (JSON format)
- Default configuration on startup

**Hardware Calibration**
- Potentiometer value mapping (raw ADC → normalized)
- Clock drift compensation
- Output level trimming per slot

---

## Performance Specifications

| Parameter | Value |
|-----------|-------|
| MIDI Input Latency | < 1ms |
| Register Update Rate | 1000 updates/second |
| UI Refresh Rate | 60 FPS (vsync) |
| Voice Allocation Time | < 0.5ms |
| Velocity Analysis Window | 1024 events |
| Supported Polyphony | 24 voices + 20 rhythm channels |

---

## Technical Implementation

**GUI Framework**: Dear ImGui (immediate-mode, single-pass rendering)
**Graphics Backend**: OpenGL 3.3 Core Profile
**MIDI Backend**: Windows Multimedia API / ALSA / CoreMIDI (platform-dependent)
**Serial Communication**: USB CDC class, 115200 baud
**Threading**: Decoupled MIDI input, hardware communication, and UI rendering threads with lock-free queues

The immediate-mode GUI architecture ensures minimal CPU overhead for real-time applications, while the modular design allows independent testing of MIDI processing, voice allocation, and hardware abstraction layers.
