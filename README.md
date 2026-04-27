# GFP-Mutation-Visualizer-Real-Time-Fluorescence-Light-Simulation-Using-WebGL-2.0
This project presents a WebGL 2.0-based real-time visualization tool for simulating the fluorescence emission of Green Fluorescent Protein (GFP) and four engineered variants: BFP (Blue), CFP (Cyan), YFP (Yellow), and RFP (Red). 

The system employs a GPU-accelerated rendering pipeline combining a modified Phong lighting model with rim-lighting and multi-pass additive blending to approximate fluorescence emission. Five distinct chromophore configurations are modeled based on established site-directed mutagenesis data at amino acid position 66 (and position 203 for YFP). A 2D spectral panel implements Gaussian-curve rendering to visualize emission spectra over 400–700 nm. Quantitative evaluation demonstrates that all variants achieve ≥55 FPS on mid-end hardware (Intel UHD 620, Chrome 124), and emission peak accuracy is within ±3 nm of published spectroscopic reference data. An additional pairwise sequence alignment module highlights the single-residue substitution (Y66H) distinguishing GFP from BFP. 

**A. Application Architecture**

The application is implemented as a pure single-page HTML file using WebGL 2.0 with zero external JavaScript dependencies. The rendering state machine is managed entirely within a single script block, eliminating network round-trips and making the application suitable for offline use. The user interface is organized into four tabs: (1) 3D View—real-time protein rendering with fluorescence glow; (2) Emission Spectrum—2D Gaussian spectral curves; (3) Sequence Alignment—pairwise GFP vs. BFP residue comparison; and (4) Mutation Analysis—directed mutagenesis table with chromophore biogenesis annotation.

**B. Fluorescence Lighting Model**

Fluorescence emission is approximated by adapting the Phong shading model with two physically motivated modifications: rim-lighting and additive multi-pass glow. Table I summarizes the three lighting components used in the fragment shaders.

<img width="1012" height="204" alt="image" src="https://github.com/user-attachments/assets/30335b95-f549-4a38-8076-6c690eedba2f" />

**D. Variant Parameter**

Each of the five GFP variants is encoded as a typed JavaScript object containing: emission wavelength λem (nm), key mutation string, RMSD of Cα backbone against wild-type GFP (Å), and RGB glow color computed from the CIE 1931 color-matching function evaluated at λem. Table II shows the complete variant parameterization.

<img width="840" height="292" alt="image" src="https://github.com/user-attachments/assets/98512ff7-fe84-4aab-a33c-0c6646ddb4a4" />

**E. Spectral Rendering**

The Emission Spectrum panel renders 2D spectral curves on an HTML5 Canvas element overlaid on the WebGL viewport. Each variant's emission profile is modeled as a Gaussian distribution with full-width at half-maximum (FWHM) of 40 nm (σ = 20 nm), centered at λem. Wavelengths are mapped to display RGB via the Bruton spectral color algorithm (400–700 nm), producing perceptually accurate hue gradients beneath each curve. The Gaussian approximation captures the primary emission peak with sufficient fidelity for visualization; the experimentally observed red shoulder of wild-type GFP (~540 nm) is acknowledged as a simplification discussed in Section V.

**F. Sequence Alignment Visualization**

The Sequence Alignment tab implements a custom pairwise alignment renderer for the GFP (PDB: 1EMA, 238 residues) and BFP (PDB: 1BFP) sequences, displayed in 10-residue blocks with color-coded mismatch highlighting. The visualization demonstrates that a single residue substitution at position 66 (Tyr→His, encoded by Y66H) is sufficient to shift emission by 59 nm (507 nm → 448 nm), providing a direct visual link between genotype and photobiological phenotype.

**G. Protein Geometry Construction**

To ensure zero external dependencies and instant load, all 3D geometry is constructed procedurally on the CPU at initialization. The β-barrel is tessellated as an 11-stranded torus mesh (~5,184 vertices), the α-helix as a cylindrical tube with longitudinal subdivisions, and the chromophore as a central glowing sphere. Vertex Buffer Objects (VBOs) are uploaded once to the GPU and reused across frames; only the rotation quaternion and glow intensity uniforms are updated per frame.

**EXPERIMENTAL RESULTS & VALIDATION**

**A. Real-Time Rendering Performance**
<img width="948" height="268" alt="image" src="https://github.com/user-attachments/assets/b2a04a94-e356-4441-9ef7-02d8ae195567" />

All five variants maintain ≥55 FPS on the target mid-end hardware, satisfying the project requirement of real-time interactivity (≥60 FPS on mid-end devices). RFP exhibits the lowest frame rate (~57 FPS) due to its higher particle count (~400 vs. ~300), consistent with its structurally more divergent chromophore (RMSD 1.20 Å). Performance headroom exists for additional visual passes on higher-end hardware; a dedicated GPU (e.g., NVIDIA GTX 1650) achieves a locked 60 FPS across all variants.

**B. Emission Spectral Accuracy**
<img width="940" height="267" alt="image" src="https://github.com/user-attachments/assets/b380f52d-e424-47ae-9864-2f3ed1c3bb67" />

Emission peak accuracy was validated by comparing the application's Gaussian model centers (λem,model) against reference values from peer-reviewed spectroscopy literature [1][2][4]. Table IV reports the comparison; all variants fall within ±3 nm of experimental values, satisfying the stated accuracy target.
