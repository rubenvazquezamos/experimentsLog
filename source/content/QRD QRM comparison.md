What areas are under active investigation?
[[Length Correction Validation]]
[[Factors affecting the diffusion coefficient of Schroeder diffusers]] 

# To do

- [x] Try PML's instead of Impedance Boundary conditions.

- [x] Integrate this setup into `QRD_QRM_comparison`

- [x] Fix repo 

- [x] Add $\delta_n$ plot for radiation correction DC plot

- [x] Check normalisation for polar QRD plots

- [x] Make 3D plot for `paramRepsQRD5`

- [x] Add information and plots for `PhaseProbeQRM5_2`

- [x] Try defining $d = l_c + l_n + h$ or  $d = l_c + l_n$. This is used in the correction for a periodic array of slits given in [[@mechelFormulasAcoustics2013]].
  
# Experiments
  
## Phase at diffuser surface
  
`PhaseProbeQRM5` 
Experimented with surface probe distance from QRM after identifying error in `mphinterp()` call. It was calling `acpr.p_t` (total pressure) instead of `acpr.p_s` (scattered pressure). QRM has 12 mm width between wells ("fins") and 12 mm panel sides (stock) and QRD has 6 mm fins with 6 mm panel sides (stock).
  
  > [!Plot]-
  > ![[Pasted image 20260309110704.png]]

`PhaseProbeQRM5_2`
The surface probe experiment in `PhaseProbeQRM5` was repeated, this time with a different panel construction style and simulation domain - QRM has 6mm fins and 6mm stock and the QRD stayed the same with 6mm fins and 6mm stock. The simulation domain has Cartesian PMLs either side of the diffuser instead of impedance boundary conditions. Some distortions in the phase profile seem to have been mitigated with these adjustments.

> [!Plot]-
> ![[phase2k 1.svg]]

`6repsphase2`
Investigation of the effect of periodicity (6 repetitions) on $\arg(R)$ at 2kHz for an $N=5$ QRD and QRM. The QRM was optimised at 2kHz. The agreement between curves is much better than in the single repetition case:

> [!Plot]-
> ![[phase2k_6reps.svg]]
> ![[phase2k_1rep 4.svg]]

## Phase Jumps

`phaseAnalysis`
Phase changes across a QRD sequence. Compares phase range and phase jumps (periodic and aperiodic).

> [!Plots]-
> ![[Phase Jumps.pdf]]

`phaseAnalysis2`
Phase changes across a QRD sequence, mapped across all wells. Compares phase range and phase jumps across all wells for all frequencies.

> [!Plots]-
> ![[N5.svg]]
> ![[N5abs.svg]]

## Radiation correction

`PhiavgQRM5` $\phi_t$ (surface porosity $h_n/d$ where $h_n$ is the slit width for well $n$ and $d$ is the distance between slits along the panel) in the radiation corrections changed to an average value across wells. We are looking for closer agreement between numerical and TMM values. The radcorr expression is for a periodic array of slits so the hope is that changing $\phi_t$ to an average value will bring quantities closer to the periodic case.

> [!Plots]- 
> ![[Diffusion Coefficient.svg]]

Below is a comparison with data from `QRM5_Build`. Not obvious which model attains better agreement between numerical and analytical:

> [!Plots]- 
> ![[DC Comparison.svg]]

# Ideas

- [ ] Try Bloch-Floquet conditions for `resonator in waveguideBGFIELD5.mph`
- [ ] Add resistive JCAL layer to top of metadiffuser to prevent evanescent coupling