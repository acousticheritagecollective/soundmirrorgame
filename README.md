# PHANTOMS IN THE SKY
## An Acoustic Heritage Game of War Without Violence

---

## 1. INTRODUCTION

**Phantoms in the Sky** is a web-based heritage gamification project that brings the acoustic history of the Abbot's Cliff Sound Mirror to life through interactive gameplay. Built using JavaScript and HTML5 technologies with Three.js for 3D visualization and Web Audio API for spatial sound, this game recreates the experience of a sound mirror listener operator during the critical years of aerial defense development, from the experimental 1920s trials through multiple 20th-century conflicts to a speculative 2025 scenario.

The project emerged from rigorous academic research conducted by The Acoustic Heritage Collective, who visited the Abbot's Cliff Sound Mirror in Kent, UK in October 2022 to conduct in-situ measurements, impulse response recordings, and acoustic heritage assessments. The game translates this scientific acoustic modeling into an accessible, educational, and emotionally engaging experience that safeguards not only the technical characteristics of the sound mirror but also the cultural memory and listening practices of a bygone era.

Unlike traditional war games that focus on violence and destruction, Phantoms in the Sky centers on *listening* as an act of defense—a game where survival depends on acute auditory perception, patience, and precision. Players assume the role of a listener operator, using a rotating acoustic horn to detect incoming aircraft formations before they breach defensive lines. The game's auralization model is based on authentic acoustic simulations that account for distance attenuation, atmospheric absorption, mirror gain characteristics, and the frequency response of period-accurate listening devices including stethoscopes and trumpet cones.

This project represents a novel approach to acoustic heritage safeguarding, moving beyond conventional impulse response documentation to create a fully interactive, historically-grounded virtual environment. By combining academic rigor with game design, Phantoms in the Sky offers an alternative method of historical media reception—one that engages players viscerally with the acoustic realities of pre-radar air defense while honoring the fears, anxieties, and listening practices of those who operated these monumental concrete structures.

---

## 2. HISTORICAL BACKGROUND

### The Abbot's Cliff Sound Mirror

The Abbot's Cliff Sound Mirror is a concrete acoustic reflector with a spherical dish 6.096 meters (20 feet) in diameter and a radius of curvature of 4.572 meters (15 feet), located on the Kent coast of the United Kingdom at coordinates 51.101696, 1.243615. Built in 1928 as part of Dr. William Sansome Tucker's "Sound Mirror Programme," this structure was one of several experimental early warning systems designed to detect approaching aircraft before the invention of radar.

The mirror was constructed following successful trials at Hythe and formed part of "the Hythe system," a network of acoustic mirrors with different designs that represented ongoing experimentation to discover which shapes and sizes worked best for aerial threat detection. The whole structure was inclined 5 degrees horizontally and extends on a NW-SE orientation (axis 165 true bearing) along the edge of the cliff.

### Original Operational Use

Sound mirrors functioned by concentrating and amplifying sound waves from distant aircraft engines. Their spherical concave surfaces converted flat waves into spherical ones at a specific focal point, creating concentric patterns that could bring distant sounds seemingly close to the listener. The experimental mirrors had a maximum detection range of 22 miles (approximately 35 kilometers), though this was significantly reduced on windy days.

At the focal point of the mirror, a listening system was positioned. Initially, this consisted of a trumpet (46-61 cm wide angle end) attached to a stethoscope. Later systems incorporated electric microphones. By rotating the listening device, the operator in the control room below the mirror could determine the direction of incoming aircraft and relay this information to anti-aircraft batteries via communication systems. The mirrors were capable of determining the incoming direction of an aircraft to within 2 degrees or less, providing the British forces with approximately 15 minutes of warning before an attack.

A 1924 report suggested that sound mirrors were ten times more sensitive than the human ear, and they were tested by blind listeners in 1925 to maximize auditory acuity. Test results indicated that operators should switch every 40 minutes to avoid irritation caused by the noise of small weather changes or the movement of ships in the English Channel.


![home](https://github.com/user-attachments/assets/ca876f7e-c897-42be-b4db-c490db591c15)



### Strategic Context and Obsolescence

The construction location of Abbot's Cliff followed recommendations from the Royal Aircraft Factory: "The position chosen for a reflector should be the flat top of a low hill since such a position is fairly free from local sounds. An absence of trees is also an advantage since the rustling of the leaves interferes with the hearing. If mounted near the coast, say on cliffs, the reflector should be kept back say two or three hundred yards from the edge of the cliffs so as to eliminate the noise of the waves."

Once constructed, the mirrors were tested using aircraft, ships, and concrete pipes that projected low-frequency drones at frequencies below 50 Hz to simulate aircraft noise. However, with the development of faster aircraft in the 1930s and the advent of radar technology, sound mirrors became obsolete before they were ever used in actual combat. They were never deployed in a real attack scenario.

### Cultural and Monumental Heritage

The Abbot's Cliff Sound Mirror is registered with Historic England as Monument Number 1413672. Beyond its technical and historical significance, the mirror holds profound cultural meaning. As R. Murray Schafer noted in his discussion of "Acoustic Space," sound mirrors remind us of an acoustic world where survival was reliant on listening skills—a monument to an era when hearing was a primary tool of defense and perception.

Paul Virilio argued in "War and Cinema" that weapons are tools of perception as well as destruction. The sound mirrors can be interpreted as monuments to the fear of the future, to the unknown, to the need for anticipation. They represent what Selina Bonelli called "a summed concretion of historic and present fears"—concrete ears crafted from industrial materials, listening for threats across the English Channel.

These structures have inspired numerous artistic works, including paintings by Eric Ravilious (1941), sound installations, experimental music compositions, video art, architectural studies, and design objects. They have become pieces of art in themselves—imposing yet graceful, simultaneously evoking silence, solitude, deep listening, and the anxieties of technological warfare.

---

## 3. GAME INSTRUCTIONS

### Objective

Your mission as the listener operator at Abbot's Cliff Sound Mirror is to detect incoming enemy aircraft formations before they breach defensive lines. You must use your acoustic horn to sweep the sky, find peak engine sound intensity, and confirm detections with precise timing.

### Controls

- **ARROW KEYS (← →)** or **MOUSE MOVEMENT**: Rotate the acoustic horn and camera view 360 degrees horizontally
- **ARROW KEYS (↑ ↓)**: Adjust vertical viewing angle (pitch control)
- **SPACE**: Confirm detection when you hear maximum signal
- **ESC**: Pause the game
- **Q**: Quit mission (returns to instructions after confirmation)
- **SHIFT + ARROW KEYS**: Fine adjustment mode (0.05° instead of 0.2° per step)

### Core Mechanics

#### **Acoustic Detection**

Aircraft formations approaching across the English Channel generate engine noise that travels through the atmosphere to your position. The sound mirror concentrates these distant sounds at its focal point, where your acoustic horn is positioned. By rotating the horn left and right, you can sweep across different angles to find the direction where engine noise is loudest.

When the horn is aligned with an incoming formation, you will hear the engine sound increase in volume and intensity. The game's audio system simulates:

- **Distance attenuation**: Aircraft far away sound quieter; closer aircraft are louder
- **Atmospheric absorption**: High frequencies are filtered more at greater distances
- **Mirror gain**: The concrete dish provides approximately +18 dB amplification at perfect alignment, decreasing as angle misalignment increases
- **Listening device frequency response**: The trumpet and stethoscope system emphasizes low frequencies (30-120 Hz) where aircraft engines produce most sound

#### **Detection Tolerance**

Each wave has a different **tolerance** value that determines how precisely you must align the horn with the formation's true direction. Early waves (1920s trials) allow wider margins of error (±1.2 degrees), while later waves demand increasingly precise alignment as operator training improved and faster aircraft required quicker responses.

Press **SPACE** when you believe the horn is optimally aligned. If you are within the tolerance range, you successfully detect the formation. If not, you miss, and the attempt goes into cooldown.

#### **Cooldown System**

After each detection attempt (successful or failed), there is a **1.5-second cooldown** before you can attempt again. This simulates the time required for the operator to process the signal, make a decision, and prepare for the next attempt. A visual cooldown bar at the bottom of the screen shows your readiness status.

#### **Lives and Breaches**

You have **3 lives per wave**. You lose a life if:

- You **miss a detection** (press SPACE when not aligned within tolerance)
- A formation **breaches** defensive lines by getting too close (within 1.5 km), escaping at extreme angles (beyond ±30°), or reaching the mirror position

When you lose all 3 lives, the wave ends, and you proceed to the next historical period (or to game over if you've completed all waves).

#### **Scoring**

Points are awarded based on:

- **Detection distance**: Detecting formations at greater distances earns more points (maximum 1000 points for detections at 40 km)
- **Streak bonus**: Consecutive successful detections increase your multiplier
- **Wave completion**: Completing all required detections in a wave grants bonus points

### The Seven Waves: Historical Periods and Challenges

#### **Wave 1: 1920s TRIALS (1928)**

- **Historical Context**: Experimental phase with slow biplanes (30 m/s / ~67 mph)
- **Weather**: Clear skies, optimal listening conditions
- **Spawn Rate**: 8 seconds between formations (leisurely pace)
- **Maximum Simultaneous**: 2 formations
- **Tolerance**: ±1.2° (generous)
- **Weather Boost**: 0 dB (no additional ambient noise)
- **Aircraft Sound**: Early 1920s engines (engine_1920s.mp3)
- **Detections Required**: 3 successful detections to advance
- **Challenge**: Learning the basic mechanics; getting accustomed to the acoustic cues

#### **Wave 2: 1939-45 BLITZ (WWII)**

- **Historical Context**: German bomber attacks during the Battle of Britain
- **Weather**: Storm conditions with heavy rain and wind
- **Aircraft Speed**: 110 m/s (~246 mph)
- **Spawn Rate**: 6 seconds
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±1.0° (more precise)
- **Weather Boost**: +5 dB (storm noise degrades signal-to-noise ratio)
- **Aircraft Sound**: WWII-era engines (engine_1940s.mp3)
- **Detections Required**: 3
- **Challenge**: Storm weather creates acoustic interference; faster aircraft approach more quickly

#### **Wave 3: 1950s KOREA**

- **Historical Context**: Early jet age; Korean War period tactical aircraft
- **Weather**: Overcast with moderate ambient noise
- **Aircraft Speed**: 280 m/s (~626 mph)
- **Spawn Rate**: 5 seconds
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±0.8°
- **Weather Boost**: +8 dB
- **Aircraft Sound**: 1950s turboprop/early jet engines (engine_1950s.mp3)
- **Detections Required**: 4
- **Challenge**: Significantly faster targets; narrower detection window

#### **Wave 4: 1979-89 AFGHAN (Soviet-Afghan War era)**

- **Historical Context**: Cold War tactical aircraft; high-altitude fast movers
- **Weather**: Haze with reduced visibility
- **Aircraft Speed**: 260 m/s (~582 mph)
- **Spawn Rate**: 4 seconds
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±0.7°
- **Weather Boost**: +10 dB
- **Aircraft Sound**: 1980s military jet engines (engine_1980s.mp3)
- **Detections Required**: 4
- **Challenge**: Rapid spawn rate; increased atmospheric noise

#### **Wave 5: 1982 FALKLANDS**

- **Historical Context**: Falklands War; subsonic and supersonic threats
- **Weather**: Fog (dense)
- **Aircraft Speed**: 300 m/s (~671 mph)
- **Spawn Rate**: 3 seconds
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±0.6°
- **Weather Boost**: +12 dB
- **Aircraft Sound**: Falklands-era jet engines (engine_1982.mp3)
- **Detections Required**: 5
- **Challenge**: Dense fog creates severe acoustic masking; very tight tolerance; high spawn rate

#### **Wave 6: 1991 DESERT STORM**

- **Historical Context**: Gulf War; modern multi-role fighters
- **Weather**: Clear skies (ironically no weather advantage despite clear conditions)
- **Aircraft Speed**: 650 m/s (~1,454 mph / supersonic)
- **Spawn Rate**: 2 seconds
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±0.5°
- **Weather Boost**: +15 dB
- **Aircraft Sound**: Modern turbofan engines (engine_1991.mp3)
- **Detections Required**: 5
- **Challenge**: Extremely fast supersonic targets; very narrow tolerance; minimal time to react

#### **Wave 7: 2025 EASTERN FRONT (Speculative Future)**

- **Historical Context**: Near-future conflict scenario with advanced stealth and hypersonic threats
- **Weather**: Severe storm
- **Aircraft Speed**: 850 m/s (~1,901 mph / hypersonic)
- **Spawn Rate**: 1.5 seconds (relentless)
- **Maximum Simultaneous**: 3 formations
- **Tolerance**: ±0.4° (elite precision required)
- **Weather Boost**: +20 dB
- **Aircraft Sound**: Advanced future engines (engine_2025.mp3)
- **Detections Required**: 6
- **Challenge**: The ultimate test—hypersonic speeds, extreme weather, minimal tolerance, and rapid-fire spawns

#### **Wave 8+: ENDLESS WAR**

After completing Wave 7, you enter an endless mode where waves continue indefinitely with progressively increasing difficulty:

- Speed increases by 50 m/s per wave
- Weather boost increases by +1 dB per wave
- Tolerance decreases by 0.03° per wave (minimum 0.15°)
- Spawn interval decreases by 0.1 seconds per wave (minimum 1.0 second)
- Detections required: 6 per wave

### Environmental Systems

#### **Weather and Atmospheric Conditions**

Each wave features different weather conditions that affect acoustic detection:

- **Clear**: Minimal interference; optimal signal propagation
- **Overcast**: Moderate ambient noise from wind and atmospheric turbulence
- **Haze**: Light scattering of sound; slight degradation
- **Fog**: Dense acoustic masking; significant signal loss
- **Storm**: Severe interference from rain, wind, and thunder; dramatic reduction in effective range

Weather is represented both visually (changing skybox colors and fog densities) and acoustically (ambient weather sounds via the Web Audio API's weather gain node). Thunder may occur randomly during storm conditions.

#### **Dynamic Backgrounds**

As you progress through waves, the equirectangular background panorama changes to reflect different historical periods and locations, synchronized with the frontmirror texture that overlays the 3D mirror model.

### HUD (Heads-Up Display)

The game interface provides essential information:

- **Wave Label** (top-left): Current wave number and name
- **Formation Status** (top-left): Number of active (undetected) formations approaching
- **Score** (top-right): Current point total
- **Lives** (top-center): Remaining lives shown as diamonds (♦)
- **Horn Angle Readout** (bottom-center): Current horn alignment in degrees; warns when outside effective mirror range (±30°)
- **Cooldown Bar** (bottom-center): Visual indicator of detection readiness
- **Radar Display** (bottom-right): Top-down tactical view showing:
  - Your position (center)
  - Active formations (red blips with distance/angle data)
  - Detected formations (dimmed blue markers)
  - Mirror field of view

### Strategy Tips

1. **Sweep Methodically**: Move the horn slowly across the horizon; rushing leads to missed peaks
2. **Listen for Maxima**: Engine sound will rise and fall as you rotate; detect at the loudest point
3. **Use Fine Control**: Hold SHIFT for 0.05° micro-adjustments when close to alignment
4. **Watch the Radar**: Use the tactical display to anticipate formation positions and angles
5. **Manage Cooldown**: Don't spam SPACE; wait for confidence before attempting detection
6. **Prioritize Threats**: In multi-formation scenarios, detect the closest/fastest threats first
7. **Adapt to Weather**: Storms and fog reduce effective detection range; rely more on radar cues
8. **Learn Aircraft Sounds**: Each era has distinct engine characteristics; familiarity improves detection speed

---

## 4. CREDITS

### Research and Field Measurements

**In-Situ Measurements at Abbot's Cliff Sound Mirror (October 2022)**
- Ginebra Raventós
- Ɇ₥łⱠłØ ₥₳ⱤӾ
- Edgardo Gómez

### Academic Research

**Original Research by The Acoustic Heritage Collective**
- Marx E.
- Raventós G.
- Gómez E.
- Lavandeira J.

Publication: *Acoustic Heritage Safeguarding of the Abbot's Cliff Sound Mirror and a proposed Auralization Model for Heritage Gamification*

### Technical Development

**Acoustic Modeling and Simulation**
- Computational modeling reconstructed using i-SIMPA free software (CSTB)
- Spherical mirror gain calculations based on in-situ measurements and theoretical acoustic principles
- Atmospheric attenuation coefficients derived from Crocker (1998) reference data

**Auralization Model Design**
- Ɇ₥łⱠłØ ₥₳ⱤӾ

**Game Design and Programming**
- Ɇ₥łⱠłØ ₥₳ⱤӾ
- JavaScript/HTML5 implementation
- Three.js 3D graphics engine
- Web Audio API spatial audio system
- Custom distance attenuation and mirror gain algorithms

**3D Modeling**
- Joan Lavandeira
- Mirror geometry based on historical dimensions (6.096m diameter, 4.572m radius of curvature)
- Environmental assets and scene composition

### Audio Assets

**Aircraft Engine Recordings**
- Period-specific engine sound samples (engine_1920s.mp3 through engine_2025.mp3)
- Reference recording: Fokker Dr.1 triplane with 9-cylinder Le Rhône 110 horsepower 9J rotary engine (Pole Position Production, 2023)

**Environmental Soundscapes**
- Field recordings: Ginebra Raventós, Ɇ₥łⱠłØ ₥₳ⱤӾ, Edgardo Gómez (Abbot's Cliff, 2022)
- Weather audio: ambient.mp3, viento.mp3 (wind), lluvia.mp3 (rain), tormenta.mp3 (storm), truenos.mp3 (thunder)
- Seagull ambience: gaviotas.mp3

### Historical Sources and References

- Scarth, R.N. (1999). *Echoes from the Sky: A Story of Acoustic Defence*. Hythe Civic Society
- Goodman, S. (2012). *Sonic Warfare: Sound, Affect, and the Ecology of Fear*. MIT Press
- Schafer, R.M. (2007). "Acoustic Space 1." *Circuit* 17(3): 83-86
- Virilio, P. (1989). *War and Cinema: The Logistics of Perception*. Verso
- Historic England Monument Number 1413672
- Wessex Archaeology archival materials
- Imperial War Museum collections

### Special Thanks

- **The Acoustic Heritage Collective** for pioneering research in acoustic heritage safeguarding
- **i-SIMPA development team** (CSTB) for providing free acoustic simulation software
- **Historic England** for heritage site preservation
- **The local community of Kent** for maintaining and protecting the Abbot's Cliff Sound Mirror
- **All artists and researchers** who have contributed to the cultural understanding of sound mirrors through their creative works

### Technology Stack

- **Three.js** (r128): 3D rendering engine
- **Web Audio API**: Spatial audio processing and synthesis
- **JavaScript ES6+**: Core game logic
- **HTML5/CSS3**: Interface and styling
- **RAF851 Font Family**: Period-appropriate typography

### Project Information

**Title**: Phantoms in the Sky — Acoustic Heritage Collective  
**Website**: [acousticheritagecollective.org](https://acousticheritagecollective.org)  
**Year**: 2026  
**Location**: Barcelona, Spain / Berlin, Germany

---

## Closing Statement

Phantoms in the Sky is dedicated to the memory of the operators who stood vigil at acoustic mirrors across the British coast, listening for threats in the darkness. Through this game, we honor their patience, skill, and the profound human capacity to defend through *listening*—a game of war without violence, where survival depends not on destruction, but on acute perception and the courage to hear what approaches from beyond the horizon.

---

*"Sound is our first sense. Before we enter the world, we can hear sounds from outside the womb. After birth, human infants can recognize the sounds they heard before embarking on their journey to the outside world. On a fundamental level, sound can help us to recognize things we don't yet know that we know."*  
— Williams & Coblentz (2017)
