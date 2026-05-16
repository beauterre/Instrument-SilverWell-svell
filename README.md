# Project: The Svell (Silverwell)
**A Hybrid FM-Sampled Resonant Instrument**

## 1. The Instrument Concept
**The Svell** (originally called the **Silverwell**) is a fictional keyboard instrument designed to bridge the gap between the precision of a harpsichord and the rich, organic resonance of a piano.

### Physical Architecture
*   **The Interface:** A standard keyboard layout, but with a "Variable Tension Pluck" mechanism.
*   **The Strings (Membranes):** Instead of steel wires, it uses **silver-plated copper wrapped around a carbon-fiber core**. These "membranes" provide a thicker, more complex vibration than a wire, creating a "bloom" of sound rather than a sharp "ping."
*   **The Action:** Each key triggers a flexible **silicone/leather plectrum**. The depth of the pluck is determined by the player's velocity, allowing for a range of expressions from a ghostly whisper to a metallic shatter.
*   **The Silverwell:** The membranes are stretched over a **deep, parabolic mahogany chamber**. This "well" contains "ghost membranes"—sympathetic strings that are never played directly but vibrate in harmony with the main notes, creating a shimmering, cathedral-like halo of sound.

---

## 2. The Sonic Architecture (FM Implementation)
To recreate this in a digital environment, the Svell uses a **Pre-Generated FM Sample** approach. This combines the mathematical complexity of FM synthesis with the stability and richness of high-fidelity sampling.

### The FM Stack (The DNA)
Each note is generated using a 4-operator stack:
1.  **The Snap (Carrier):** High-frequency transient for the initial pluck.
2.  **The Fundamental:** The core pitch.
3.  **The Membrane (Modulator 1):** Set to a **non-integer ratio (1.414)** to create the inharmonic, organic "skin" texture.
4.  **The Shimmer (Modulator 2):** High-ratio modulation to simulate the metallic silver coating.

### The Expression Layers
Every key is rendered into three distinct samples based on velocity:
*   **Ghost (ppp):** Low modulation index. Ethereal, soft, and breathy.
*   **Lyric (mf):** Balanced modulation. The signature "Svell" tone—warm and rich.
*   **Shatter (ff):** High modulation index. Aggressive, metallic, and harmonically complex.

---

## 3. Technical Implementation Roadmap (WebFM)

### Stage A: The Bake-and-Store Pipeline
Instead of real-time synthesis, an `OfflineAudioContext` is used to "bake" the instrument:
*   **Matrix:** 88 Keys×3 Expression Layers=264 unique buffers\text{88 Keys} \times \text{3 Expression Layers} = 264 \text{ unique buffers}88 Keys×3 Expression Layers=264 unique buffers.
*   **Export:** Buffers are exported as compressed `.opus` or `.wav` files.
*   **Manifest:** A JSON map links MIDI notes to their respective sample paths.

### Stage B: The Playback Engine
The browser-side engine handles the performance using the **Web Audio API**:
*   **Dynamic Morphing:** Cross-fading between Ghost →\rightarrow→ Lyric →\rightarrow→ Shatter based on MIDI velocity.
*   **Swell Envelope:** A custom ADSR where the Modulators bloom slightly after the Carriers hit.
*   **The "Silverwell" DSP:** 
    *   **Convolution Reverb:** Applying an Impulse Response (IR) of a wooden chamber.
    *   **Sympathetic Bus:** A low-level "ghost" layer that triggers resonant harmonics of the played note.
    *   **Wooden Filter:** A dynamic Low-Pass filter that closes as the note decays to simulate wood absorption.

### Stage C: User Experience
*   **Interface:** A virtual keyboard with a "Swell Ribbon" (morph slider).
*   **Control:** A "Well Depth" knob to adjust the convolution wet/dry mix.
