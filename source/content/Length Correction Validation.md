# Theory

For length corrections explanation see [[@jaouenLengthCorrection2D2018]]. In our research they are used for changes of section in waveguides, although they can be used for more than this, including as end corrections (but not for slit geometries).
For radiation correction for periodic slits formula see [[@mechelFormulasAcoustics2013]].
Perhaps it's worth trying some entirely different models? As in [[@ramosModellingEffectiveSound2025]].

# Experiments

To validate the length corrections I propose adapting the methodology in [[@ramosModellingEffectiveSound2025]].

`wavegHRslit`
First approach to comparing TMM and numerical results for a slit with a side-branch HR. Doesn't feature retrieval of $R$ from numerical scattered pressure. That's the next step. $S_w$ is the cross section of the main waveguide that the slit is loaded into in series. Note that although the plot states that the numerical probe is at $x = L$ (the input point of the slit resonator system), it is really *near* to $L$, as putting it at $L$ would mean it coincides with the rigid boundary.

> [!Plots]-
> ![[geo.svg]]
> ![[impedance.svg]]
> ![[reflectionCoefficient 1.svg]]
> ![[scatteredPressure 4.svg]]

# Models

The repository containing the various versions of these models is LengthCorrectionValidation

## TMM model

The total transfer matrix is given as
$$
\mathbf{T = M_{\Delta l_{slit}} M_s M_{HR} M_s}
$$
where 
$$
\mathbf{M_s}= 
\begin{bmatrix}
\cos(k_s \frac{a_y}{2}) & iZ_s\sin(k_s \frac{a_y}{2}) \\
\frac{i}{Z_s} \sin(k_s \frac{a_y}{2}) & \cos(k_s \frac{a_y}{2})
\end{bmatrix}
$$
is the transfer matrix for the half slit segment,
$$
\mathbf{M_{HR}} = \begin{bmatrix}
1 & 0 \\
1/Z_{HR} & 1
\end{bmatrix}
$$ 

is the transfer matrix for the Helmholtz Resonator loaded in parallel in the waveguide and

$$
\mathbf{M_{\Delta l_{slit}}}= 
\begin{bmatrix} 
1 &Z_{\Delta l_{\mathrm{slit}}} \\
0 &1
\end{bmatrix}
$$

is the transmission matrix for an element loaded in series in the waveguide.

The impedance of the HR (with length corrections, and after performing Taylor expansion) is given by 

$$
Z_{HR}= - i Z_n
  \frac
  {
  \cos k_n l_n \cos k_c l_c 
  -\frac{k_n \Delta l Z_n}{Z_c}
  \cos k_n l_n \sin k_c l_c -\frac{Z_n}{Z_c} \sin k_n l_n \sin k_c l_c
  }
  {
  \sin k_n l_n \cos k_c l_c -\frac{k_n \Delta l Z_n}{Z_c} \sin k_n l_n \sin k_c l_c + \frac{Z_n}{Z_c} \cos k_n l_n \sin k_c l_c
  }
$$

  where $Z_n = \sqrt{\beta_n / \rho_n}/S_n$ , $Z_c = \sqrt{\beta_c / \rho_c}/S_c$ are the effective acoustic impedances for the neck and waveguide expressed in flow formulation. The cross sections are defined as $S_n=w_n$ and $S_c = w_c$ for a system with 1D propagation. Lastly, the length correction is given as the sum of two smaller corrections $\Delta l = \Delta l_1+\Delta l_2$.

$$
\Delta l _{1} =0.82\bigg[ 1- 1.35 \frac{R_n}{R_c}+0.31\bigg( \frac{R_n}{R_c} \bigg)^3\bigg]R_n
$$

is the length correction due to a simple discontinuity from [[@kergomardSimpleDiscontinuitiesAcoustic1987,]]

  $$
  \Delta l_2 = 0.82 
  \bigg[ 1-0.235\frac{R_n}{R_s}-1.32\bigg( \frac{R_n}{R_s}\bigg)^2+1.54 \bigg( \frac{R_n}{R_s}\bigg)^3-0.86\bigg(\frac{R_n}{R_s} \bigg)^4
  \bigg]R_n
  $$

  is the length correction for a branch in a tube from [[@dubosTheorySoundPropagation1999]],

$$\Delta l _{\mathrm{slit}} =0.82\bigg[ 1- 1.35 \frac{R_s}{R_t}+0.31\bigg( \frac{R_s}{R_t} \bigg)^3\bigg]R_s$$

is the length correction due to the simple discontinuity between the slit and the main waveguide, where $R_c =w_c/2$, $R_n=w_n/2$ and $R_s=h/2$ $R_t=d/2$ is half the width of the main waveguide; $d = l_c+l_n + h$. Both types of correction are printed as in [[@theocharisLimitsSlowSound2014]]. Note that the factor $0.82$ in front of the expressions can appear as $0.41$ when the inputs are full widths as in [[@jimenezRainbowtrappingAbsorbersBroadband2017]]. The radiation impedance of the slit is given as 

$$
Z_{\Delta l_{\mathrm{slit}}} = 
\frac{
-i \omega \Delta l_{\mathrm{slit}} \rho_0
}{
\phi_t S_s
}
$$

Similar to [[@jimenezRainbowtrappingAbsorbersBroadband2017]], where the slit cross section is $S_s=h$ for a system with 1D propagation and the perforation ratio is $\phi_t = h/d$.

## COMSOL .mph model

COMSOL models have been made with a variety of waveguide configurations and formulations for the impedance $Z$ and reflection coefficient $R$. For an alternative model with propagation direction defined as $-y$ direction see [[resonator in waveguideBGFIELD5.mph]]. The model described here is `resonator in waveguideBGFIELD6.mph`. This model centers the slit in the waveguide and defines the propagation and waveguide axes as the $+x$ direction

> [!Geometry]-
> ![[Pasted image 20260309134846.png]]
> ![[Pasted image 20260309135139.png]]

$Z_{spec} = p/v_y$ this impedance is different depending on where you position the point probe.
and therefore the reflection coefficient is $R = \frac{Z_{spec}-Z_0}{Z_{spec}+Z_0}$ 

> [!Plot]-
> ![[Pasted image 20260309135403.png]]

This reflection coefficient is computed more directly as $R_1=\int p_{ref} \ dx/ \int p_{inc} \ dx$ , with the $y$ coordinate the input of the slit

> [!Plot]-
> ![[Pasted image 20260309135921.png]]

$Z_1=\frac{\int p \ dx}{\int v_x \ dx}$ ,with the $y$ coordinate the input of the slit. The reflection coefficient is computed as $R_2 = \frac{Z_1-Z_0}{Z_1+Z_0}$ 

> [!Plot]-
> ![[Pasted image 20260309135908.png]]

## COMSOL .m model

Based on `resonator in waveguideBGFIELD6.mph`. It does not feature any of the "retrieval methods" for $R$ and $Z$ of the latter model, instead it queries the scattered pressure using `mphinterp` and leaves the computation of propagation quantities to MATLAB.

# Questions

- Why don't my "retrieval methods" for $R$ all agree? Both for analytical and numerical. It might have something to do with the impedance expression in the TMM. 