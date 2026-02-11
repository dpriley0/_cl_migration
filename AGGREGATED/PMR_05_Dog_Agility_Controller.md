# Project Migration Record: Remote-Controlled Dog Agility Course Obstacles

---

# PART I: COMPREHENSIVE PROJECT SUMMARY

## 1. Project Vision

### High-Level Goals
Build a system of remote-controlled obstacles for dog agility courses that can be positioned and adjusted via a smartphone app, allowing trainers to modify course layouts without entering the field.

### Motivations
- **Dynamic Course Adjustment**: Modify course layout mid-session without entering the field
- **Progressive Training**: Gradually increase difficulty by moving obstacles as dog's skills improve
- **Automated Training Sequences**: Program specific movement patterns for consistent training
- **Personal Project**: Hardware/software integration learning opportunity

### Apps, Tools, Utilities, and Services of Interest
- **ESP32-WROOM-32 Microcontroller** - Main device controller
- **Motor Controllers** - For obstacle movement
- **Li-ion Battery System** - Rechargeable power
- **WiFi/Bluetooth Communication** - Phone-to-device control
- **Mobile App (iOS/Android)** - Remote control interface
- **Position Tracking System** - Course mapping via smartphone triangulation

### Strategic Objectives
1. Build prototype remote-controlled obstacle mover
2. Develop companion smartphone app for control
3. Implement position recording functionality
4. Create basic course mapping using smartphone-based triangulation
5. Test in real agility course environment

### Long-term Potential / Potential Workflows

**Training Workflow:**
1. Trainer sets up initial course manually
2. Records obstacle positions via app
3. During training, adjusts obstacles remotely without entering field
4. Saves course configurations for future sessions
5. Progressively modifies difficulty as dog improves

**Course Setup Workflow:**
1. Walk around course perimeter to establish reference points
2. App creates coordinate system
3. Place obstacles and record positions
4. Quickly recall saved configurations for standard courses

### Unique Value Proposition
Enables hands-free course modification during agility training sessions, improving training efficiency and dog-handler connection.

### Core Philosophies Discussed
- **Start simple**: Basic movement and control first
- **Iterate based on real use**: Test with actual agility training
- **Leverage smartphone capabilities**: Use phone sensors for mapping instead of expensive hardware
- **Safety first**: Low-speed operation, obstacle avoidance consideration

---

## 2. Requirements Compilation

### Explicit Requirements

**Hardware:**
- Motors with sufficient torque to move obstacles (without being dangerously fast)
- Rechargeable Li-ion batteries (weight not critical)
- WiFi or Bluetooth connectivity to smartphone
- Low-profile design to avoid tripping hazards

**Performance:**
- Operating range: <200 ft typical, 500-700 ft maximum
- Motor usage: Short 30-45 second bursts
- Idle power: Only maintaining wireless connection when "on"
- Response time: Quick enough for mid-session adjustments

**App Features:**
- Directional controls (forward/back/left/right)
- Speed adjustment
- Position recording and recall
- Battery level indicator
- Course map visualization

### Implied Requirements
- Weather resistance (outdoor use)
- Durable construction (dog training environment)
- Safe operation around dogs and handlers
- Easy charging access
- Visual/audible low battery warnings

### Derived Requirements
- ESP32-WROOM-32 provides adequate processing and built-in WiFi/Bluetooth
- Bluetooth 5.0+ sufficient for <200ft range (up to 800ft theoretical)
- No internet connectivity needed (direct phone-to-device)
- Position tracking via RSSI with ~1-2 meter accuracy

### Potential Edge Cases or Expansion Areas
- Obstacle avoidance sensors
- Multiple obstacle coordination
- Automated sequence programming
- Camera integration for remote viewing
- Integration with training tracking apps

---

## 3. Architectural Overview

### Current Design Decisions with Rationale

**ESP32-WROOM-32 Microcontroller (CHOSEN)**
- Built-in WiFi and Bluetooth capabilities
- Low power modes for battery efficiency when idle
- Plenty of GPIO pins for motors, sensors, peripherals
- Readily available libraries for motor control and communication
- Actually overpowered for this use case (room to expand)

**Bluetooth 5.0+ Primary Communication (PLANNED)**
- Sufficient range for typical use (<200ft)
- Lower power than WiFi
- Simple pairing
- WiFi as backup for extended range if needed

**Smartphone-Based Position Triangulation (PLANNED)**
- No additional beacon hardware required
- Uses RSSI (Received Signal Strength Indicator)
- 1-2 meter accuracy (adequate for prototype)
- Smartphone sensors (accelerometer, gyroscope, compass) can enhance accuracy

### Decisions We Considered and Discussed

**Positioning Approaches Evaluated:**
1. **Manual with relative tracking** (simplest) - Record positions manually, track relative movements
2. **Fixed beacon triangulation** - 2-3 beacons at known positions, more accurate but more hardware
3. **Camera-based** - Overhead camera with ArUco markers, complex but user-friendly
4. **GPS-based** - Ruled out: consumer GPS lacks precision for agility course scale
5. **UWB (Ultra-Wideband)** - Very accurate but expensive and overkill

**Selected Approach**: Smartphone-based RSSI triangulation with ~1-2m accuracy, adequate for prototype

### Technology Stack (Planned)
- **Microcontroller**: ESP32-WROOM-32
- **Motors**: Servo or gear motors (TBD based on obstacle weight)
- **Battery**: Li-ion 18650 cells with BMS
- **Communication**: Bluetooth 5.0+ (primary), WiFi Direct (backup)
- **Mobile App**: Flutter (cross-platform iOS/Android)

### Key Architectural Constraints
- No internet/cloud connectivity (direct phone-to-device only)
- Battery-powered operation
- Outdoor/field environment
- Safety around dogs and people
- <200ft typical operating range

### Potential Future Directions
- Multiple obstacle coordination
- Automated training sequences
- Integration with training analytics
- Video streaming capability
- Voice control integration

---

## 4. Current State

### What's Been Designed ✅
- Overall system architecture
- Hardware platform selection (ESP32-WROOM-32)
- Communication approach (Bluetooth primary)
- Position tracking strategy (smartphone RSSI)
- App feature requirements
- Development phases outlined

### What's Been Built
- Nothing yet - project is in planning/design phase

### Next Steps to Start
1. Acquire ESP32-WROOM-32 development board
2. Select and acquire motors and motor drivers
3. Design basic prototype chassis
4. Set up development environment
5. Build simple movement test

---

# PART II: PROJECT ROADMAP AND BACKLOG

## Mission Control Dashboard 🚀🛠️

### Purpose
This backlog tracks development of remote-controlled dog agility course obstacles - a hardware/software project combining ESP32 microcontrollers with a smartphone control app.

### Current Status: Design Complete, Hardware Acquisition Needed

---

## Development Phases

### Phase 1: Prototype Hardware
- [ ] Acquire ESP32-WROOM-32 development board
- [ ] Select motors (servo or gear) based on obstacle weight
- [ ] Acquire motor controllers/drivers
- [ ] Design and build prototype chassis
  - Low profile to avoid tripping
  - Rubber wheels for varied surfaces
  - Space for batteries and electronics
- [ ] Set up Li-ion battery system with BMS
- [ ] Assemble single-obstacle test unit
- [ ] Basic movement testing

### Phase 2: Basic App Development
- [ ] Set up Flutter development environment
- [ ] Implement Bluetooth communication
- [ ] Create basic directional controls UI
- [ ] Test bidirectional communication with ESP32
- [ ] Add speed adjustment
- [ ] Add battery status display

### Phase 3: Enhanced Features
- [ ] Implement position recording functionality
- [ ] Add position recall/goto feature
- [ ] Create basic course layout visualization
- [ ] Add preset position buttons for quick access
- [ ] Implement smooth acceleration/deceleration

### Phase 4: Position Mapping System
- [ ] Implement RSSI-based position estimation
- [ ] Develop calibration routine
- [ ] Create course mapping interface
- [ ] Build reference point registration flow
- [ ] Test accuracy in various environments
- [ ] Refine with smartphone sensor enhancement

---

## Hardware Components List

### Core Electronics
| Component | Purpose | Notes |
|-----------|---------|-------|
| ESP32-WROOM-32 | Main controller | Built-in WiFi/BT |
| Motor driver (TBD) | Motor control | Based on motor selection |
| Motors (TBD) | Movement | Servo or gear |
| Li-ion 18650 cells | Power | Multiple for capacity |
| BMS module | Battery management | Charging, protection |
| Power regulator | Voltage regulation | 5V/3.3V for electronics |

### Mechanical
| Component | Purpose | Notes |
|-----------|---------|-------|
| Chassis/frame | Structure | Low profile design |
| Wheels (rubber) | Movement | Various surface compatibility |
| Motor mounts | Mechanical coupling | |
| Enclosure | Protection | Weather-resistant |

### Optional/Future
| Component | Purpose | Notes |
|-----------|---------|-------|
| Proximity sensors | Obstacle avoidance | Safety feature |
| Status LEDs | Visual feedback | Battery, connection |
| Buzzer/speaker | Audio feedback | Warnings |

---

## Technical Specifications

### Performance Targets
- **Typical range**: <200 ft
- **Maximum range**: 500-700 ft
- **Motor operation**: 30-45 second bursts
- **Position accuracy**: 1-2 meters (adequate for prototype)

### Power Budget (Estimated)
- **Idle (connected)**: ~50-100mA
- **Motor operation**: ~500mA-2A (depends on motor)
- **Target battery life**: Multiple training sessions between charges

### Communication Protocol (Conceptual)
```
Phone → ESP32: {command: "move", direction: "forward", speed: 0.5}
ESP32 → Phone: {status: "ok", battery: 85, position: {x: 5.2, y: 3.1}}
```

---

## Timeline Estimate
- **Research and component selection**: 2 weeks
- **Initial prototype assembly**: 4 weeks
- **Basic app development**: 4 weeks
- **Integration and testing**: 2 weeks
- **Enhanced features and position mapping**: 4-6 weeks

**Total**: 16-18 weeks (4-4.5 months) for functional prototype

---

## Success Criteria

**MVP Success:**
- Single obstacle moves reliably via phone control
- Basic directional controls work
- Position recording/recall functions
- Battery lasts through training session

**Full Success:**
- Multiple obstacles coordinated
- Position mapping accurate to ~1-2 meters
- Reliable outdoor operation
- Used in actual agility training sessions

---

# PART III: CONVERSATION CONTEXT SUMMARY

## Key Discussion Points
1. **Simplified requirements**: Weight not critical, no internet needed, short bursts of motor use
2. **ESP32 selection**: Overpowered for the use case but provides room to grow
3. **Positioning approach**: Smartphone RSSI triangulation avoids extra hardware cost
4. **1-2 meter accuracy**: Adequate for prototype agility course use

## Notable Insights
- Bluetooth 5.0 has theoretical range of 800ft in ideal conditions
- WiFi antenna might be unnecessary with modern Bluetooth
- Position tracking without additional hardware beacons is feasible
- Project complexity reduced significantly with "no internet" requirement

## Unresolved Questions
- Exact motor selection (depends on obstacle weight)
- Chassis design specifics
- Weatherproofing approach
- How to make it obvious that it's safe around dogs

## Potential Pivot Points
- Could add camera for remote viewing
- Could integrate with training tracking apps
- Could commercialize if successful

## What I've Learned About This Project
- Related to Laura's equestrian activities (connection to horse/dog training world)
- Dan has hardware experience with ESP32 and Li-ion systems
- The "no internet" requirement significantly simplifies architecture
- Smartphone-as-positioning-device is a clever cost/complexity reduction

## Additional Context
- This is a "future" project, not immediately prioritized
- Demonstrates Dan's interest in hardware/software integration
- Could tie into broader home automation interests

---

## Related Projects
- **AstralJunction Home Lab**: Potential for device management dashboard
- **AI-augmented Project Management**: Could track this hardware project

## Migration Notes
This PMR consolidates the complete design discussion for the agility course obstacle controller. The project is fully scoped but not yet started. Key technical decisions (ESP32, Bluetooth, RSSI positioning) are documented and ready for implementation when prioritized.
