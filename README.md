# GFP-Mutation-Visualizer-Real-Time-Fluorescence-Light-Simulation-Using-WebGL-2.0
This project presents a WebGL 2.0-based real-time visualization tool for simulating the fluorescence emission of Green Fluorescent Protein (GFP) and four engineered variants: BFP (Blue), CFP (Cyan), YFP (Yellow), and RFP (Red). 

The system employs a GPU-accelerated rendering pipeline combining a modified Phong lighting model with rim-lighting and multi-pass additive blending to approximate fluorescence emission. Five distinct chromophore configurations are modeled based on established site-directed mutagenesis data at amino acid position 66 (and position 203 for YFP). A 2D spectral panel implements Gaussian-curve rendering to visualize emission spectra over 400–700 nm. Quantitative evaluation demonstrates that all variants achieve ≥55 FPS on mid-end hardware (Intel UHD 620, Chrome 124), and emission peak accuracy is within ±3 nm of published spectroscopic reference data. An additional pairwise sequence alignment module highlights the single-residue substitution (Y66H) distinguishing GFP from BFP. 

A. Application Architecture
The application is implemented as a pure single-page HTML file using WebGL 2.0 with zero external JavaScript dependencies. The rendering state machine is managed entirely within a single script block, eliminating network round-trips and making the application suitable for offline use. The user interface is organized into four tabs: (1) 3D View—real-time protein rendering with fluorescence glow; (2) Emission Spectrum—2D Gaussian spectral curves; (3) Sequence Alignment—pairwise GFP vs. BFP residue comparison; and (4) Mutation Analysis—directed mutagenesis table with chromophore biogenesis annotation.

B. Fluorescence Lighting Model
Fluorescence emission is approximated by adapting the Phong shading model with two physically motivated modifications: rim-lighting and additive multi-pass glow. Table I summarizes the three lighting components used in the fragment shaders.

<img width="1012" height="204" alt="image" src="https://github.com/user-attachments/assets/30335b95-f549-4a38-8076-6c690eedba2f" />
