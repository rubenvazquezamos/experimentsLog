# Introduction

Investigation with particular focus on $N=5$ QRD and the disagreement between numerical and analytical (TMM) predictions of the diffusion coefficient. This was noticed for the first time in the experiment `QR5_TMM_COMSOL`. This is the original experiment which notes the discrepancy in analytical (TMM) and numerical (COMSOL) computations of the diffusion coefficient $\delta$ around the 1 kHz 3rd octave band for an $N = 5$ QRD :

![[QR5_TMM_COMSOL.svg]]

The diffusion coefficient itself is not the only quantity considered in order to try to explain this discrepancy; other quantities include the phase at the well surface $\arg(R)$ and the polar distribution of scattered acoustic pressure, $Ps$.
# Experiments

A number of hypotheses were considered with the overarching goal of explaining and/or fixing the discrepancy in the 1/3rd octave band centered at 1kHz. The first key observation was that we CAN make the models agree if the well width is sufficiently wide. See `1kQRDagree`. This still unexplained observation led to the hypotheses around panel repetitions and the accuracy of the flat panel models. Below is the list of hypotheses tested so far:

**Hypothesis 0:** The analytical (TMM) model is inaccurate. **Conclusion:** we cannot discard this hypothesis altogether, but the TMM predictions are at least as good as those found in the literature. See section *Analytical Diffusion Coefficient*, which considers the effect of radiation corrections, angle restrictions and type of scattering integral.

**Hypothesis 1:** There is some kind of normalisation issue when performing the calculation for the normalised diffusion coefficient. i.e. The flat panel is not being simulated correctly in COMSOL. See `flatPanelFarField`,  `flatPanelFarField_2` and `RSfarfield2`. **Conclusion:** Flat panels behave as expected, and moreover their performance can be predicted to a good degree of accuracy in the near field as well as far field.

**Hypothesis 2:** The diffusion coefficient will become more accurate if the correct kind of normalisation is performed. See `1kQRDagree2` where numerical $\delta_n$ is calculated using far field flat panel scattering. **Conclusion:** normalising to a far field flat panel scattering pattern doesn't improve agreement.

**Hypothesis 3:** The discrepancy at 1k is caused by lack of periodicity. More specifically by the wells at the edge not having another well to interact with (we might call this "pseudo-periodicity").
**Conclusion:** This doesn't seem to be the case. See the section *Panel Repetitions*, in particular `paramRepsQRD5`. It shows that the phase agreement improves when periodicity is present, but the diffusion coefficient agreement does not.

**Hypothesis 4:** The discrepancy could be due to simulation domain issues such as the type of boundaries chosen (PMLs/Impedance Boundary conditions) or even geometric issues in the simulation. See the section *Simulation Domain Considerations*, in particular `CartPML_full`.
**Conclusion:** No major simulation domain issues have been identified.

**Hypothesis 5:** Fin width is distorting the scattered field. **Conclusion:** It isn't. See `QRD5fins`.

**Hypothesis 6:** Boundary conditions at the edge of the diffuser are the problem. A "Baffled" (flush with the PML) and diffuser extruded 30cm above the PML were compared. **Conclusion:** it makes little difference, See `UnBaffledQRD5`.

**Hypothesis 7:** evanescent coupling could be causing the issue. **Conclusion:** Nothing conclusive but see below a plot of sound intensity and pressure at 1kHz. It seems to show the field behaving as expected:
>[!Plot]-
>![[Evanescent Coupling Detection 1.jpeg]]

**Hypothesis 8:** The model is not "tuned" correctly. Small discrepancies might be adding up. **Conclusion:** the scattered pressure plots were matched very closely to those in Jiménez et al 2017 and this was found to not fix the issue. See the section *Tuning*.
## Analytical Diffusion Coefficient

`QR5_radcorr`
The first thought was maybe radiation corrections could have been influencing the TMM $\delta$, so the two conditions were compared:

> [!Plots]-
> ![[QR5_radcorr.svg]]

`DC_factors`
Next, the researcher attempted a general replication of the TMM $\delta$ , comparing obtained results with those in the literature [[@ballesteroOverhaulingSoundDiffusion2021]], and varying the parameters radiation correction ($\mathrm{on, off}$) and grazing angle restriction ($1,5,10^{\circ}$):

> [!Plots]-
> ![[QR5_DC_TMM2 1.svg]]
> 
> ![[QR5_DC_TMM3.svg]]

`DC_factors2`
A further [existing result](https://ericballestero.github.io/acoustic_metamaterials.html) from was added to the comparison and perfect agreement reached:

> [!Plots]-
> ![[TMM3&EricWeb.svg]]

The TMM model was considered good enough for the time being.

`QRD5RaylSommer_full`
Diffusion coefficient using Rayleigh Sommerfeld integral formulation compared those obtained using Fraunhofer integral. Results are similar:
>[!Plots]-
>![[QRD5RaylSommer_full_DC_comparison.svg]]

## Agreement at 1kHz

`1kQRDagree`
The discrepancy at 1kHz was investigated in more detail, this time by varying well widths ($7,10,15,20 \mathrm{cm}$) and observing the effect on the diffusion coefficient in the 800-1.2kHz range. The effect on scattered pressure at 1kHz of probe distance ($3,10\mathrm{m}$), and angle resolution ($181, 31 \mathrm{pts}$) was also observed:

> [!Plots]-
> ![[BroadbandDC-W20cm 1.svg]]
> 
> ![[DC(W).svg]]
> 
> ![[ProbeDist 1.svg]]
> 
> ![[theta_resolutionfig 1.svg]]

`1kQRDagree2`
Normalisation was explored as an avenue to obtain better agreement. The hypothesis was that better results could be obtained by computing $\delta_n$  numerical results using a $\delta_{flat}$ derived from the TMM model, which is a Fraunhofer far field approximation and therefore represents an idealised flat plane scatterer. Agreement did not improve:

> [!Plot]-
> ![[Normalisation.svg]]

No effect was observed on the scattered pressure at 1kHz when changing the PML scaling, $(1, 6)$:

> [!Plot]-
> ![[PMLScaling.svg]]

Negligible effects, most likely due to the change in overall panel length were observed on the scattered pressure at 1kHz when changing the "fin" width, which is the width of material between wells, ($1.2 ,12 \mathrm{mm}$):

> [!Plot]-
> ![[PolarFins1kHz.svg]]

`QRD5fins`
The effect of fin width on the diffusion coefficient of an $N=5$ QRD with 7cm well widths from 250Hz-4kHz was evaluated for two different fin widths: 0.5 and 12 mm. The thin fin panel had an overall width of 0.353 m and the fat fin panel an overall width of 0.422 m, which accounts for the difference in Fraunhofer TMM predictions for each panel (which don't feature fins), and probably for the slight change in numerical $\delta$ too. It's also notable to observe that the $\arg(R)$ presents hyperbolic divergence around the well edges for the thin well case but not for the fat case:

> [!Plot]-
> ![[DC 2.svg]]
> ![[phase 1.svg]]
> ![[polar2k.svg]]

## Panel repetitions

`6repsQRD5`
The effect of adding 6 panel repetitions on the scattered pressure at the design frequency of $500 \,\mathrm{Hz}$ was observed. The expected $N$ (5) far field grating lobes were observed. note that grazing angles were restricted by 5 degrees: 

> [!Plot]-
> ![[polar.svg]]

`paramRepsQRD5`

This experiment studies the effect panel repetitions ($1, 2, 3, 4, 5, 6$) on the normalised diffusion coefficient $\delta_n$. The simulation probe was in the far field at all times, according to the criterion that the observer should be at a distance at least 3 times greater than the largest wavelength of interest and 3 times larger than the size of the panel. The simulation domain size starts at 3m, with care was taken to make the sure the was domain was at least 3x bigger than the panel size and the largest wavelength of interest:

> [!Plot]-
> ![[DC.svg]]
> ![[DCbyreps3D_num.svg]]
> ![[DCbyreps3D_TMM.svg]]
> ![[phaseByReps.svg]]

## Simulation domain considerations

`QRD5CartPML`
Swapped impedance BCs with cartesian PMLs. Scattered pressure compared to plot in [[@jimenezMetadiffusersDeepsubwavelengthSound2017]]

> [!Plot]-
> ![[Pasted image 20260313145757.png]]


`7repsPsHeatmap`
Used 7 periods of QRD instead of 6 to compare like-for-like with [[@jimenezMetadiffusersDeepsubwavelengthSound2017]] (it seems 7 periods are being used in the paper).

> [!Plot]-
> ![[7repPsHeatmap.png]]


`CartPML_full`
Swapped impedance BCs with cartesian PMLs. Compared with results using different simulation boundary conditions. The effect on flat panel simulations was also considered and compared to TMM results. There are some differences at low frequencies.

> [!Plot]-
> ![[comparison.svg]]
> 
> See below for comparison of flat panel data
> 
> ![[comparisonFlat.svg]]

`UnBaffledQRD5` 
A "Baffled" (flush with the PML) and diffuser extruded 30cm above the PML were compared.
>[!Plots]-
>![[DC 1.svg]]
>![[DC_norm.svg]]
>Extruded:
>![[geoUnbaffled.svg]]
>"Baffled":
>![[geoBaffled.svg]]
## Flat panel scattering

`flatPanelFarField`
Numerical flat panel scattering as a function of domain size. Probe is 1m from domain edge at all times to avoid interference from PMLs. Panel length is approx. $1.9$ m:

> [!Plot]-
> ![[polarAsFunctionofDomainSize 1.svg]]

`flatPanelFarField_2`
Same as above but with 50m domain size to check convergence to far field pattern. Simulation for 50m domain took 72 minutes.

> [!Plot]-
> ![[polarAsFunctionofDomainSize50m.svg]]

`RSfarfield2`
TMM Flat panel scattering using Rayleigh Sommerfeld integral compared with far field formulation (Fraunhofer integral). Indicates that much of the changes in polar pattern with distance can be accounted for by using the right scattering integral.
>[!Plot]-
>![[RSfarfield2_comparison.svg]]

## Tuning

`QRD5Noe`

The scattered pressure field shown in figure 3(e) of Jiménez et al. (2017) was not exactly matching my model. Jiménez used 7 panel repetitions. Matching of figure 3(e) in Jiménez et al. (2017) was carried out by tuning $Ps$ as function of $\epsilon_{fin}$ , $W$ etc. The settings were reverse engineered to be $\epsilon_{fin} \approx 1$ mm $W \approx 6$ cm and  $2\epsilon_{stock} = 2\epsilon_{fin}$ . See pdf for full set of comparisons:

>[!pdf]-
>NOTE: pdf doesn't display correctly in Obsidian. Download the file to view properly
>![[e_fin and W.pdf]]
>Animation of $Ps(W)$:
>![[e_fin1.gif]]

Then, using the obtained settings, a parametric study of $\delta$ and $\delta_n$ as a function of $W$ was performed:
>[!Plot]-
>![[DC_comparisonQRD5Noe.svg]]
>![[DCnorm_comparisonQRD5Noe.svg]]