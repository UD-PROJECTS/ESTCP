
Centre Module
******************************

The central module solves the Boussinesq equations and also takes care basic functions such as wavemaker, wave breaking, spongelayers, boundary conditions and model input and output.

Boundary Conditions
======================

  **Wall boundary condition**
     A mirror boundary condition is used for a fully reflective wall.

  **Periodic boundary condition**
     The periodic boundary condition in *y* (south/north) direction was implemented in the code.

Wave Breaking, roller and undertow
===================================

================
Breaking schemes
================

There are two breaking algorithms implemented in the model. One takes advantage of the shock--capturing scheme in TVD. 
It  follows the approach of `Tonelli and Petti (2009) <https://www.sciencedirect.com/science/article/pii/S0378383909000027>`_,  who successfully used the ability of the nonlinear shallow water equation (NSWE) with a TVD scheme to model moving hydraulic jumps. Thus, the fully nonlinear Boussinesq equations are switched  to NSWE at cells where the Froude number exceeds  a certain threshold. Following Tonelli and Petti, the ratio of wave height to total water depth is chosen as  the criterion to switch from Boussinesq to NSWE, with a threshold value  set to 0.8,  as suggested by Tonelli and Petti. 

The other wave breaking scheme is the original eddy--viscosity scheme used in the previous version of FUNWAVE (`Kennedy et al., 2000 <https://pdfs.semanticscholar.org/e5fc/6de940e793517a2835cd8a11743f36fc2cfe.pdf>`_. To fit the eddy--viscosity method in the TVD scheme, the artificial eddy viscosity terms are:

.. math:: {\bf R}_{bx} = \frac{\partial }{\partial x} (\nu \frac{\partial P}{\partial x}) + \frac{\partial }{\partial y} (\nu \frac{\partial P}{\partial y} )

.. math:: {\bf R}_{by} = \frac{\partial }{\partial y} (\nu \frac{\partial Q}{\partial y}) + \frac{\partial }{\partial x} (\nu \frac{\partial Q}{\partial x}) 

Note that the form is slightly different from that in Kennedy et al. (2000). The present form was found to give a more stable numerical solution with the cross--derivatives removed. In the present form, :math:`\nu` is the artificial eddy viscosity defined by:

.. math:: \nu = B \delta_b^2 (h+\eta) \eta_t

where :math:`\delta_b = 1.2`. In Kennedy et al. (2000), :math:`B` varies smoothly from 0 to 1 so as to avoid an impulsive start of breaking and the resulting instability. In the present TVD model, because there is no instability problem found, we adopt a constant value :math:`B=1` as breaking is initiated:

.. math:: B =  1 \ \ \  \eta_t \ge  \eta_t^* 
.. math:: B =  0 \ \ \  \eta_t <  \eta_t^*

The parameter :math:`\eta_t^*` determines the onset and cessation of breaking. Following Kennedy et al., a breaking event begins when :math:`\eta_t` exceeds some initial threshold value :math:`\eta_t^{(I)}`, as breaking develops, the wave will continue to break until :math:`\eta_t` drops below :math:`\eta_t^{(F)}`. However, we do not use the smooth transition as in Kennedy et al. because the present TVD scheme did not encounter any instability problem associated with breaking. The values of :math:`\eta_t^{(I)}` and :math:`\eta_t^{(F)}` can be described by  :math:`C_{brk1}  \sqrt{gh}` and  :math:`C_{brk2} \sqrt{gh}`, respectively, where :math:`C_{brk1}` and  :math:`C_{brk2}` are empirical parameters. In Kennedy et al., :math:`C_{brk1} = 0.65` and  :math:`C_{brk2}=0.15`. `Choi et al. (2018) <https://www.sciencedirect.com/science/article/pii/S1463500318301793?via%3Dihub>`_ showed that :math:`C_{brk1}` should be smaller and :math:`C_{brk2}` be larger than those in Kennedy et al. to match the laboratory experimental data. For the benchmark test of `Vincent and Briggs (1989) <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-950X(1989)115:2(269)>`_ for instance, :math:`C_{brk1} = 0.45` and :math:`C_{brk2} = 0.35` were adopted. 

===================
Roller and undertow
===================

.. figure:: images/guide/funwave/roller.jpg
    :width: 500px
    :align: left
    :height: 250px
    :alt: alternate text
    :figclass: align-left

*Figure 1. Concept of roller (from Schäffer et al., 1993). Cross-section and assumed velocity profile of a breaking wave with a surface roller.*

**Roller-induced flux**

The general expression of a breaking roller in a Boussinesq-type model was introduced by several authors such as Madsen (1981), `Svendsen (1984) <https://www.sciencedirect.com/science/article/pii/0378383984900280>`_, and `Schäffer et al. (1993) <https://www.sciencedirect.com/science/article/pii/037838399390001O>`_. For a one-dimensional case, the roller induced mass flux can be expressed by:

.. math:: P=u_0d + (c-u_0) \delta,  \ \ \ \ when \ \ breaking
.. math:: P=u_0d, \ \ \ \ \ \ \ \ \ \ else

where :math:`P` is the total mass flux including the contribution of roller. :math:`u_0` is the velocity defined in the figure, :math:`c` is the wave celerity, and :math:`\delta` is the roller thickness. In FUNWAVE-TVD, we use :math:`u_\alpha` to represent the velocity :math:`u_0`. :math:`c` is calculated using:

.. math:: c = \sqrt{gd}   

which is different from Schäffer et al. (1993) who used the local still water depth: :math:`c=1.3\sqrt{gh}`. 

The thickness of roller :math:`\delta` can be estimated using the roller geometry shown in Figure 1. However, in the parallelized program, locating the roller region involves cross-core-boundary tracking, that is nontrivial and time-consuming. In FUNWAVE-TVD, we used a rough estimate of the roller thickness based on the correlation between the roller area and the wave height proposed by Svendsen (1984), i.e. :

.. math:: A = 0.9 H^2 
   :label: svendsen

where :math:`A` represents the roller area and :math:`H` is the wave height. Based on the roller geometry, the roller area can be estimated as: 

.. math:: A = \frac{LH}{2} r   
   :label: geometry

where :math:`L` is the wave length, :math:`r` is a ratio representing the thickness, and :math:`\delta = rH`. Assuming the wave length can be estimated by :math:`L = 4 H /\tan \theta`, where :math:`\tan \theta` is estimated by :math:`\tan \theta = \eta_t/c`. According to :eq:`svendsen` and :eq:`geometry`, the ratio :math:`r` can be calculated by:

.. math:: r = 0.45 \tan \theta  

The ratio :math:`r` is limited by the maximum breaking angle (:math:`20^{\circ}`, Schaffer et al. 1993), resulting in the maxumim value of :math:`r = 0.1638`.
 
We further assume the local thickness of the roller at the breaking point is :math:`\delta = r (\eta^*-\bar{\eta})`, where :math:`\eta^{*}` and :math:`\bar{\eta}` are the surface elevation at a breaking point and the mean surface elevation, respectively.  The final formula for the roller-induced mass flux can be expressed as:

.. math:: P=u_0d + 0.45 (c-u_0) \tan \theta (\eta^{*}-\bar{\eta}),  \mbox{ at  breaking  point}
   :label: flux

The mean surface elevation is calculated using the time series of surface elevation before the roller estimation. 

**Roller effect on hydrodynamics**

Following Schaffer et al. (1993), the total momentum flux, including the roller contribution, can be expressed as:

.. math:: M = \int^\eta_{-h} u^2 dz = u_0^2 d +(c^2-u^2_0) \delta
   :label: momentum

The excess momentum effect due to the non-uniform velocity distribution can be calculated using :eq:`flux` and :eq:`momentum`:

.. math:: R = M-P^2/d

or 

.. math:: R = (c-u_0)^2 \delta (1-\frac{\delta}{d})

The roller effect on hydrodynamics can be calculated by adding extra terms, :math:`R_x` and :math:`R_y`, in the momentum equations in the x and y directions, respectively.  

The calculation of the undertow uses the local balance of the roller induced momentum flux and the undertow flux. The roller/undertow effect is taken into account in the sediment transport processes. 

*References*

Choi, Y.-K., Shi, F., Malej, M., and Smith, J. M., 2018, "Performance of various shock-capturing-type reconstruction schemes in the Boussinesq wave model, FUNWAVE-TVD", Ocean Modelling, 131, 86-100. `DOI:10.1016/j.ocemod.2018.09.004 <https://doi.org/10.1016/j.ocemod.2018.09.004>`_. 

Kennedy, A.B., Chen, Q., Kirby, J.T., Dalrymple, R.A., 2000. "Boussinesq modeling of wave transformation, breaking and runup. I: 1D". J. Waterway Port Coastal Ocean Eng. 126(1), 39–47.

Madsen, P.A. 1981. "A model for a turbulent bore". Series paper 28, Inst. Hydrodyn. and Hydraul. Engng, Tech. Univ. Denmark.

Schäffer H. A., Madsen, P.A., Deigaard, R., 1993, A Boussinesq model for waves breaking in shallow water, Coastal Engineering, `DOI:10.1016/0378-3839(93)90001-0 <https://doi.org/10.1016/0378-3839(93)90001-O>`_

Svendsen, LA., Wave Heights and Set-Up in a Surf Zone, 1984, Coastal Engineering, Vol. 8. `DOI:10.1016/0378-3839(84)90028-0 <https://doi.org/10.1016/0378-3839(84)90028-0>`_

Tonelli, M., and M. Petti, 2009. "Hybrid finite volume -- finite difference scheme for 2DH improved Boussinesq equations". Coastal Engineering, 56(5-6), 609-620. `DOI:10.1016.j.coastaleng.2009.01.001 <https://doi.org/10.1016/j.coastaleng.2009.01.001>`_

Vincent, C.L., Briggs, M.J., 1989. "Refraction-diffraction of irregular waves over a mound". J. Waterway Port Coastal Ocean Eng. 115 (2), 269–284. `DOI:10.1061/(ASCE)0733-950X(1989)115:2(269) <https://doi.org/10.1061/(ASCE)0733-950X(1989)115:2(269)>`_

Sponge Layer
===================================

The sponge layer technique introduced by `Larsen and Dancy (L--D type, 1983) <https://www.sciencedirect.com/science/article/pii/0378383983900224>`_ is implemented in the code. In this method, the variables :math:`\phi` (i.e., :math:`\eta, u, v`) are directly attenuated at every time step:

.. math:: \phi = \phi /C_s

where :math:`C_s` is a damping coefficient function defined by:

.. math:: C_s = \alpha_s^{\gamma_x^{i-1}}, \ \ \ \ \ i=1,2, ..., n

in which :math:`\alpha_s` and :math:`\gamma_s` are two free parameters. :math:`i` represents grid numbers. `Chen et al. (1999) <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-950X(1999)125:4(176)>`_ suggested that :math:`\alpha_s =2`, :math:`\gamma_s = 0.88 - 0.92`, and :math:`n=50 - 100`. **The length of the sponge layer is usually taken to be one or two times the typical wavelength.** Chen et al. also pointed out that the damping coefficients for optimal absorption are somewhat case--sensitive. 

Recently, some problem was found in application of L--D type sponge layer for long--term simulations. The direct damping method combined with the TVD scheme generates sawtooth noises with a :math:`2 dx` wave length. The sawtooth noises are usually not noticeable due to small magnitudes. However, they grow gradually with time and may become significant in a long term simulation. If this problem occurs, we suggest using the following friction--type or viscous--type sponge layers. Using the combination of L--D and friction/viscous sponge layers may remove the sawtooth noises and also make the wave damping more efficient. 


Friction type and Diffusion type

The friction--type and viscous type sponge layers directly use the friction terms and diffusion terms existing in the model. The source term for the friction--type sponge can be described as:

.. math:: 
  F_{frc} = - C_{sponge} |{\bf u_\alpha}|  (u_\alpha, v_\alpha) h

Note that depth :math:`h` is added in the formula above to make the source term depth--independent in terms of the flux--type momentum equations. For the diffusion--type sponge, the description of diffusion term follows exactly the eddy--viscosity breaking formulation with spatial varying viscosity coefficients :math:`\nu_{sponge}`.  Both coefficients are smoothly ramped in space at the sponge layer boundaries. For example, for a sponge layer on the left end of the domain, :math:`C_{sponge}` can be written as:

.. math:: C_{sponge} = C_{max} \left (1-  \mbox{tanh} \frac{10 (i-1)}{I_{\mbox{width}}-1} \right)

where :math:`C_{max}` is the maximum value of :math:`C_{sponge}` used in the sponger layer. :math:`i` and :math:`I_{\mbox{width}}` represent the point number and the layer width in points, respectively. Similar expressions can be obtained for sponge layers on three other ends of the domain as well as the viscous sponge layer. 

The width of the sponge layer is usually taken to be two or three wave lengths for the friction--type and viscous sponge layers. Narrow sponge layers can be used for L--D type sponge layer with a good efficiency but sawtooth noise generated by the method is a concern for long--term simulation. 

*References*

Chen, Q., Madsen, P.A., Basco, D.R., 1999. "Current Effects on Nonlinear Interactions of Shallow--Water Waves". J. of Waterway, Port, Coastal, and Ocean Eng. 125 (4).

Larsen, J. and Dancy, H., 1983. "Open boundaries in short wave simulations -- A new approach". Coastal Eng. 7 (3), 285-297.

Wave--maker
===================================

There are two primary types of numerical wave--makers: Internal and Boundary. In this section, we will discuss internal wave--maker theory.

**Internal wave--maker theory**


The internal wavemaker was implemented based on `Wei et al. (1999) <https://www.sciencedirect.com/science/article/pii/S0378383999000095>`_ two--way internal wavemaker and `Chawla and Kirby (2000) <https://www.sciencedirect.com/science/article/pii/S0141118700000055>`_ one--way internal wavemaker (under development). Here, we briefly summarize the formulations used in the wavemakers. Detailed theory can be found in Wei and Kirby (1999) and Chawla and Kirby (2000).    

Wei and Kirby (1999) followed the approach of `Larsen and Dancy (1983) <https://www.sciencedirect.com/science/article/pii/0378383983900224>`_ who used an ad--hoc source mechanism where water mass is added and subtracted along a straight source/sink line inside the computing domain. This approach works well in a staggered--grid differencing scheme, where water is essentially being added to or drained from a single grid block. In applying this technique to the Boussinesq model on an unstaggered grid, however, Wei and Kirby found that use of a single source line accused high frequency noise, leading to blowup of the model. They then used a partially distributed mass source :math:`f(x,y,t)`:

.. math:: f(x,y,t) = g(x) s(y,t)

where :math:`g(x)` is a Gaussian shape function and :math:`s(y,t)` the input time series of the magnitude of source function with an assumption that the center of the source region is parallel to the y--axis. The functions :math:`g(x)` and :math:`s(y,t)` are defined as:

.. math:: g(x) = \mbox{exp}[-\beta(x-x_s)^2]

.. math:: s(y,t) = D \mbox{sin} (\lambda y -\omega t)
 
where :math:`\beta` is the shape coefficient for the source function, and :math:`x_s` is the central location of the source in the :math:`x` direction, for a source oriented parallel to the :math:`y` axis, as shown in the figure below. :math:`D` is the magnitude of the source function, :math:`\lambda = k \mbox{sin} (\theta)` the wavenumber in the :math:`y` direction, and :math:`k` is the linear wavenumber. 

For a monochromatic wave or a single wave component of a random wave train, the magnitude :math:`D` of source function can be determined by:

.. math:: D = \frac{2 a_0 \cos (\theta) (\omega^2 - \alpha_1 g k^4 h^3) }{\omega k I [1-\alpha(kh)^2]}

where :math:`\alpha = -0.390, \alpha_1 = \alpha + 1/3`, and :math:`I` is the integral given by:

.. math:: I = \int^\infty_{-\infty} \exp (-\beta x^2) \exp (-ilx) dx = \sqrt{\frac{\pi}{\beta}} \exp(- l^2/4\beta)

where :math:`l=k\cos (\theta)` is the wavenumber in the :math:`x` direction. In theory, the shape coefficient :math:`\beta` can be any number. The larger the value of :math:`\beta` is, the narrower the source function becomes. The definition of the source function width :math:`W` is not unique, and here we define :math:`W` to be the distance between two coordinates :math:`x_1` and :math:`x_2` where the corresponding source function heights are equal to :math:`\exp (-5) = 0.0067` times the maximum height :math:`D`. Then :math:`x_1` and :math:`x_2` must satisfy the quadratic equation:

.. math:: \beta (x-x_s)^2 = 5

from which the width of source function is given by:

.. math:: W = |x_2 - x_1| = 2\sqrt{\frac{5}{\beta}}

In the previous version of FUNWAVE (`Kirby et al., 1998 <http://resolver.tudelft.nl/uuid:d79bba08-8d35-47e2-b901-881c86985ce4>`_), it is suggested that :math:`W` equals about half of the wavelength for monochromatic wave. If :math:`L` is the wavelength, the requirement of :math:`W=\delta L/2` (where :math:`\delta` is of order 1) results in:

.. math:: \beta = \frac{5}{(\delta L/4)^2} = \frac{80}{\delta^2 L^2}

For random waves, the value of :math:`\beta` is determined according to the peak frequency component and then used for all components in the wave train. FUNWAVE--TVD follows the criteria for determining :math:`\beta`, though a narrow :math:`W` does not seem to cause any problem. 


.. figure:: images/guide/funwave/wavemaker.jpg
    :width: 300px
    :align: center
    :height: 250px
    :alt: alternate text
    :figclass: align-center

For the irregular wavemaker, an extension was made to incorporate an  alongshore periodicity into wave generation,  in order to eliminate a boundary effect on wave simulations. The technique exactly follows the strategy in `Chen et al. (2003) <https://agupubs.onlinelibrary.wiley.com/doi/pdf/10.1029/2002JC001308>`_, who adjusted the distribution of wave directions in each frequency bin to obtain alongshore periodicity. This approach is effective in modeling of  breaking wave--induced nearshore circulation such as alongshore currents and rip currents. 

Regular wave generation

The generation of monochromatic wave using the internal wavemaker is straightforward. Following the formulations given in 3.7.1, the magnitude of source function :math:`D` is calculated by Equation 13 shown above for given wave amplitude :math:`a_0`, wave angle :math:`\theta`, water depth :math:`h` and wave period :math:`T=1/2\pi\omega`. The source function can be obtained using the Source function above. 


Irregular wave generation


* Using directional spectral data

Irregular waves can be generated by integrating wave components split by frequency and direction and with random phases. Each wave component contains wave amplitude :math:`a_0` converted from wave energy, wave angle :math:`\theta` and wave period :math:`T`. *The source function for each component can be obtained using the source function.* 


* Using analytical spectrum function

The input for the wavemaker can be wave bulk parameters or directional spectral data. TMA shallow--water spectrum, JONSWAP spectrum and a wrapped--normal directional--spreading function are used to simulate a directional sea state. The combined spectrum function can be expressed as:

.. math:: S(f,h,\theta) = E(f,h) G(\theta)

:math:`E` is the energy density distribution as follows:

.. math:: E (f,h) = \alpha g^2 f^{-5} (2 \pi)^{-4} \Phi (2\pi f, h) e^{-5/4(f/f_p)^{-4}} \gamma^{e^{[-(f/f_p -1)^2 /2\sigma^2]}} 

in which :math:`f_p` is the peak frequency.  :math:`\gamma` presents a frequency spreading parameter, and :math:`\alpha` and :math:`\sigma` are coefficients which may be found in `Bouws et al. (1985) <https://agupubs.onlinelibrary.wiley.com/doi/pdf/10.1029/JC090iC01p00975>`_ :math:`\alpha` is obtained using the input :math:`H_{mo}/H_{sig}`:

.. math:: \sigma = 0.07  \ \ \ \  f \leq f_p 

.. math:: \sigma = 0.09  \ \ \ \  f > f_p


:math:`\Phi` = 1.0 for the JONSWAP spectrum. For TMA, :math:`\Phi` may be expressed as:

.. math:: \Phi (2 \pi f, h) =\frac{1}{2} \omega_h^2 \ \ \ \  \omega_h \leq 1

.. math:: \Phi (2 \pi f, h) = 1-\frac{1}{2}(2-\omega_h)^2 \ \ \ \ 2 > \omega_h >1
.. math:: \Phi (2 \pi f, h) = 1    \ \ \ \  \omega_h \geq 2

where, 

.. math:: \omega_h = 2 \pi f (\frac{h}{g})^{1/2}


Here, :math:`G(\theta)` is the wrapped normal directional spreading function written as:

.. math:: G(\theta) = \frac{1}{2\pi} +\frac{1}{\pi} \sum^N_{n=1} e^{[-\frac{( \sigma_{\theta})^2}{2}]} \cos n\theta 

where :math:`\sigma_{\theta}` denotes circular deviation of the wrapped normal
spreading function. To avoid the computational underflow, :math:`N = 20` in the model.

In the spectral wavemaker, the directional spectrum is first divided into :math:`1000` frequency components and then reconstructed into a user--specified number of components with the equal energy. The directional components are evenly split in each frequency. The source function technique (Wei, et al., 1999) is then used for each component and the final surface elevation function can be written as:

.. math:: \eta = \sum^M_{m=1} C_m \cos \omega _m t + \sum^M_{m=1} S_m \sin \omega _m t

where,

.. math:: C_m  = \sum^k_{n=1} D_{mn} \cos (k_{mn}y + \varepsilon_{mn})

.. math:: S_m =  \sum^k_{n=1} D_{mn} \sin (k_{mn}y + \varepsilon_{mn})

in which y--axis is oriented along the main axis of the wave maker. :math:`D_{mn}, k _{mn}` and :math:`\varepsilon_{mn}` are the amplitude, wave number in the y direction and phase of a component, respectively. The phase can be random. 

The model also provides an option for 1--D spectral wave generation (uni--directional). 


*References*

Bouws, E., Günther, H., Rosenthal, W., Vincent, C.L., 1985. "Similarity of the Wind Wave Spectrum in Finite Depth Water: 1. Spectral Form". J. of Geophysical Research, 90, NO. C1, 975-986. DOI: 10.1029/JC090iC01p00975.

Chawla, A., and Kirby, J.T., 2000. "A source function method for generation of waves on currents in Boussinesq models". App. Ocean Research, 22 (2), 75-83. DOI: 10.1016/S0141-1187(00)00005-5.

Chen, Q., Kirby, J.T., Dalrymple, R.A., Shi, F., Thorton, E.B., 2003. "Boussinesq modeling of longshore currents". J. of Geophysical Research, 108, NO. C11, 3362. DOI: 10.1029/2002JC001308

Kirby, J.T., Wei, G., Chen, Q., Kennedy, A.B., Dalrymple, R.A., 1998. "Funwave 1.0: Fully Nonlinear Boussinesq Wave Model -- Documentation and User's Manual". Hydraulic Eng. Reports: NO. CACR-98-06. University of Delaware.

Salatin, R., Chen, Q., Bak, A. S., Shi, F., and Brandt, S. R., 2021, Effects of Wave Coherence on Longshore Variability of Nearshore Wave Processes, Journal of Geophysical Research - Ocean,  `DOI:1029/2021JC017641 <https://doi.org/10.1029/2021JC017641>`_

Wei, G., Kirby J.T., Sinha, A., 1999. "Generation of waves in Boussinesq models using a source function method". Coastal Eng. 36 (4), 271-299. DOI: 10.1016/S0378-3839(99)00009-5

Tide and Surge Module
************************

There are two types of boundary conditions developed to incorporate tidal level and/or tidal current into model boundaries. One is an absorbing tidal boundary condition, which is essentially a sponge layer but takes into account a reference value, such as tidal level and tidal current, in the sponge regime. The other a so-called absorbing-generating boundary condition, which treats tidal level and/or tidal current as the background elevation/flow.  

Theory

The basic technique follows the sponge layer theory introduced by `Larsen and Dancy (1983). Instead of attenuating the surface elevation and flow velocity to zero at the end of a sponge layer, we dampen shortwaves with respect to a reference level based on the method proposed by `Chen et al. (1999) <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-950X(1999)125:4(176)>`_. The dependent variables (:math:`\eta, u, v`) are attenuated as

.. math:: \eta_i = \eta_{ref} + (\eta_i - \eta_{ref})/C_s

.. math:: u_i = u_{ref} + (\eta_i - u_{ref})/C_s

where :math:`( )_{ref}` denotes the reference values which either tidal level or tidal current or both. :math:`C_s` is the damping coefficient which is the same as that used in a sponge layer (see sponge layer section for detail), i.e., 

.. math:: C_s = \alpha_s^{\gamma_s^{(i-1)}}

in which :math:`i` is grid numbers, (:math:`i = 1, 2, ...`). 

Absorbing tidal boundary condition


For the absorbing tidal boundary condition, the reference values (:math:`\eta_{ref}, u_{ref}, v_{ref}`) are tidal levels and tidal current velocities specified either in input.txt or in a separate tide/surge file.  

For a constant tidal condition, (:math:`\eta_{ref}, u_{ref}, v_{ref}`) are constant, which are specified in input.txt. For example, in the case of tide\_abs\_1bc\_constant in the directory of simple cases, 

 .. code-block:: rest

  TIDAL_BC_ABS = T 
  TideBcType = CONSTANT 
  TideWest_ETA = 1.0 
  TideEast_ETA = 1.0 

This is a 1D case with a tidal absorbing condition and constant tidal level specified above. The wavemaker can be any types of wavemaker available in the model. In this case, we used  WK\_REG,

 .. code-block:: rest

  WAVEMAKER = WK_REG 
  DEP_WK = 8.0  
  Xc_WK = 150.0 
  Yc_WK = 0.0  
  Tperiod = 8.0  
  AMP_WK = 0.5  
  Theta_WK = 0.0  
  Delta_WK = 3.0 

The figure shows a snapshot of surface elevation from the model. Note that the irregularity of wave surface is caused by the tidal propagation from both boundaries. 

.. figure:: images/guide/funwave/tide_snap.jpg

   Output from the case of tidal absorbing boundary condition.

For a time-varying tidal condition, the tidal level and velocity are specified in a file named in input.txt. For example, in the 2D case of tide\_abs\_2bc\_data (same folder), tidal elevations are specified in two files, tide\_data\_west.txt, tide\_data\_east.txt,  with the parameter TideBcType  set as DATA type,

 .. code-block:: rest

  TIDAL_BC_ABS = T 
  TideBcType = DATA
  TideWestFileName = tide_data_west.txt 
  TideEastFileName = tide_data_east.txt

The format of tidal data follows a list of 'time, eta, u, v' as shown in the figure below.  In this case, a flat bottom of 8 m is applied in a 2D domain and a regular wavemaker is specified . The following figure demonstrates 2D and 1D section views of surface elevation at different times. Black solid lines denote tidal levels. 

.. figure:: images/guide/funwave/layout_tide_abs_only.jpg 
   :width: 550px
   :align: center
   :alt: alternate text
   :figclass: align-center

   Layout of tidal absorbing boundary (west and east).


.. figure:: images/guide/funwave/tide_abs_only.jpg
   :width: 550px
   :align: center
   :alt: alternate text
   :figclass: align-center

   Case: /simple\_cases/tide\_abs\_2bc\_data/. Demonstration of 2D and 1D section views of surface elevation at different times. Black solid lines denote tidal levels.


Combined tidal and absorbing-generating boundary condition


The combined tidal and absorbing-generating boundary condition incorporates the solution of the linear wave theory and tidal elevation and velocity in the sponge layer. 

The reference values (:math:`\eta_{ref}, u_{ref}, v_{ref}`) are tidal levels and tidal current velocities specified either in input.txt or in a separate tide/surge file. Different from the tidal absorbing boundary condition, the reference values (:math:`\eta_{ref}, u_{ref}/v_{ref}`) combine the tidal condition and wave solution, and specified over the entire computational domain. Inside the sponge layer, the differences between the reference  values :math:`( )_{ref}` and model solution :math:`( )_i` are dampened by the sponge. Outside the sponge layer, independent variables are calculated directly from the model because :math:`C_s` is 1.0. In this study, the west-side absorbing-generating boundary condition is implemented. 

An example is provided In /tide\_gen\_abs\_data/.  Figure \ref{layout_gen} shows the model setup with a west-side absorbing-generating boundary condition. In input.txt

 .. code-block:: rest

  WAVEMAKER = ABSORBING_GENERATING 
  WAVE_DATA_TYPE = DATA
  WaveCompFile = wave_data.txt 
  ... 
  TIDAL_BC_GEN_ABS = T 
  TideBcType = DATA 
  TideWestFileName = tide_data_west.txt

The format of tidal data is the same as the tidal absorbing boundary condition. The model is set up in a 2D sloping beach domain. Figure \ref{gen_abs} shows snapshots of surface elevation at different times. 

.. figure:: images/guide/funwave/layout_tide_gen_abs.jpg
   :width: 550px
   :align: center
   :alt: alternate text
   :figclass: align-center

   Layout of generating and absorbing boundary (left only)


.. figure:: images/guide/funwave/tide_gen_abs.jpg
   :width: 550px
   :align: center
   :alt: alternate text
   :figclass: align-center

   Case: /simple\_cases/tide\_gen\_abs\_data/. Thick dashed lines represent tidal levels. Thin black line denotes the beach slope.


More information


List of parameters for tidal module setup can be found `here <https://fengyanshi.github.io/build/html/tide_surge.html>`_


*References*

Chen, Q., Madsen, P.A., Basco, D.R., 1999. “Current Effects on Nonlinear Interactions of Shallow–Water Waves”. J. of Waterway, Port, Coastal, and Ocean Eng. 125 (4).

Larsen, J. and Dancy, H., 1983. “Open boundaries in short wave simulations – A new approach”. Coastal Eng. 7 (3), 285-297. DOI: `10.1016/0378-3839(83)90022-4 <https://doi.org/10.1016/0378-3839(83)90022-4>`_.


Sediment Transport Module
*****************************

Suspended Sediment Transport Equation (Non-cohesive)
=======================================================

The sediment transport module calculates sediment transport induced by both suspended load and bedload. The morphological module calculates the bed evolution based on the sediment continuity equation.

Suspended sediment motion is governed by the depth-averaged sediment concentration equation as follows:

.. math:: (\bar{c} H)_t + \nabla_h \cdot (\bar{c} H ({\bf u}_\alpha + \bar{{\bf u} }_2)) =\nabla_h \cdot (k H (\nabla_h \bar{c})) + P - D \label{ad}

where :math:`\bar{c}` is the non-dimensional depth-averaged sediment concentration normalized by sediment density. :math:`H(\bf{u}_\alpha + \bar{\bf{u}}_2) =M` represents the flow rate per unit width defined in `Shi et al. (2012) <http://www.sciencedirect.com/science/article/pii/S1463500311002010>`_, in which :math:`H=h+\eta` is the total water depth. The roller-induced extra undertow can be taken into account as an option. :math:`k` is the horizontal sediment diffusion coefficient calculated by the formula given by `Elder (1959) <https://www.cambridge.org/core/services/aop-cambridge-core/content/view/310194D66B91765946845BB274E59F7F/S0022112059000374a.pdf/dispersion_of_marked_fluid_in_turbulent_shear_flow.pdf>`_:

.. math:: k = 5.93 u_{*c} H

where :math:`u_{*c}` is the shear velocity and can be calculated by `van Rijn (1984) <10.1061/(ASCE)0733-9429(1984)110:10(1494)>`_:

.. math:: u_{*c} = \frac{\kappa}{-1 + \log (30 H / k_s)} U_c

in which :math:`U_c` is the depth-averaged total velocity (m/s), :math:`k_s = 2.5 d_{50}` is Nikuradse roughness coefficient, and :math:`d_{50}` is the median grain diameter (mm).  
 
In the advection-diffusion equation, :math:`P` and :math:`D` represent the erosion rate and deposition rate, respectively. The erosion rate can be calculated using van Rijn's (1984) pickup function:

.. math:: P = 0.015 \frac{d_{50}}{a} \left ( \frac{|\tau_b| - \tau_{cr}}{\tau_{cr}}\right )^{1.5} d^{-0.3}_{*} w_f 
    :label: p

where :math:`a` is a reference elevation and is a function of total depth (:math:`a = 0.01 H`), :math:`\tau_b` is the bed shear stress, and :math:`\tau_{cr}` is the critical shear stress. :math:`P` has the dimension of velocity (m/s) considering the convection-diffusion equation for non-dimensional sediment concentration. :math:`d_{*}` is dimensionless grain size defined as:

.. math:: d_{*} = d_{50} \left( \frac{(s-1)g}{\nu^2} \right)^{1/3}

where :math:`s` is the specified gravity of the sediment, and :math:`\nu` is the kinematic viscosity coefficient. The critical bed shear stress :math:`\tau_{cr}` used in :eq:`p` is defined as:

.. math:: \tau_{cr} = \rho_w (s-1)gd_{50} \theta_{cr}

where :math:`\theta_{cr}` is the critical Shields parameter, approximately equal to 0.05. Based on the roughness estimate, the shear stress is expressed as:

.. math:: \tau_b = \rho_w \left(\frac{ 0.4}{1+\ln (k_s/30 h)} \right)^2 U_c^2

The deposition rate :math:`D` can be calculated using the formula of `Cao (1999) <https://ascelibrary.org/doi/pdf/10.1061/%28ASCE%290733-9429%281999%29125%3A12%281270%29>`_:

.. math:: D = \gamma c w_f (1-\gamma \bar{c})^{m_o}

where :math:`\gamma = \min [2,(1-n/\bar{c})]`, :math:`n` is the sediment porosity, and :math:`m_0` is a constant number given as 2.0. 

*References*

Cao, Z. (1999) "Equilibrium near-bed concentration of suspended sediment". *J. of Hydraulic Eng.*, 125(12): 1270-1278. DOI: 10.1061/(ASCE)0733-9429(1999)125:12(1270).

Elder, J.W. (1959). "The dispersion of marked fluid in turbulent shear flow". *J. of Fluid Mech.*, 5(4): 544-560. DOI: 10.1017/S0022112059000374.

Shi, F., J.T. Kirby, J.C. Harris, J.D. Geiman, and S.T. Grilli, (2012). "A high-order adaptive time-stepping TVD solver for Boussinesq modeling of breaking waves and coastal inundation". *Ocean Modelling*, 43-44: 36-51. DOI: 10.1016/j.ocemod.2011.12.004.

Suspended Sediment Transport Equation (Cohesive)
=======================================================

The transport equation for cohesive sediment uses the same 2D form of advection and diffusion equation. 

 There are two sets of formulas to calculate the erosion rate. For hard bed, `Partheniades' (1965) formula <https://cedb.asce.org/CEDBsearch/record.jsp?dockey=0013640>`_ is used:

.. math:: P = E \left(\frac{\tau_b}{\tau_{cr}} -1 \right)

For soft bed, `Parchure and Mehta's (1985) formula  <https://ascelibrary.org/doi/10.1061/%28ASCE%290733-9429%281985%29111%3A10%281308%29>`_ is applied:

.. math:: P = E e^{\alpha \sqrt{\tau_b-\tau_{cr}}}

where :math:`E` is the erodibility specified by users, :math:`\tau_b` is the bed shear stress, and :math:`\tau_{cr}` is the critical shear stress for erosion.  The bed shear stress can be calculated using Soulsby et al. (1993):


.. math:: \tau_b = \rho_w \left(\frac{ 0.4}{1+\ln (k_s/30 h)} \right)^2 U_c^2

which is the same as for the non-cohesive sediment, and the critical bed shear stress is usually specified by users. For soft bed, :math:`\alpha` is a so-called alpha-coefficient specified by users. 

The erosion rate, :math:`P`, has the dimension of velocity (m/s) considering the convection-diffusion equation for non-dimensional sediment concentration. 

The deposition rate :math:`D` can be calculated using the formula of `Krone (1962) <https://www.worldcat.org/title/flume-studies-of-the-transport-of-sediment-in-estuarial-shoaling-processes-final-report/oclc/8967084>`_:

.. math:: D = w_s c_b p_d  
   :label: depo

where :math:`w_s` is the settling velocity which can be evaluated using a number of formulas from different sources and usually based on laboratory experiments. It should be related to processes of flocs, aggregate dimensions, drag, local concentration, salinity and other environmental factors. Users can define their own formulas by modifying the sediment module. Here, we provide a general formulation that can describe the evolution in conditions of flocculation (In `Kombiadou and Krestenitis, 2014 <http://dx.doi.org/10.5772/51061>`_):

.. math:: w_s = \frac{a \bar{c}^n}{(\bar{c}^2 + b^2)^m}

The coefficients have a large range, differing in various estuarine and riverine areas. :math:`a=0.01-0.23, b=1.3-25.0, n=0.4-2.8` and :math:`m=1.0-2.8`. The default values in the model are
:math:`a=0.1;
b=2.0;
n=0.5;
m=1.5.`
For :math:`\bar{c}=0.1 g/l`, for example, :math:`w_s = 3.9E^{-3} m/s`. 


In :eq:`depo`, :math:`c_b` is the near-bed concentration calculated by

.. math:: c_b = \beta \bar{c}

in which :math:`\beta` is the parameter. By default, we use :math:`\beta = 1`. It can also be specified by

.. math:: \beta = 1+\frac{P_e}{1.25+4.75 p_d^{2.5}}

where :math:`P_e` is the Peclet number:

.. math:: P_e = \frac{6 w_s}{\kappa u_{*c}}

in which :math:`\kappa` is von Karman constant and :math:`u_{*c}` is the friction velocity which can be calculated by `van Rijn (1984) <10.1061/(ASCE)0733-9429(1984)110:10(1494)>`_:

.. math:: u_{*c} = \frac{\kappa}{-1 + \log (30 H / k_s)} U_c

:math:`P_d` is the probability of deposition defined by

.. math:: P_d = 1- \left( \frac{\tau_b}{\tau_{cd}} \right)

where :math:`\tau_{cd}` is the critical shear stress for deposition defined by users. 

**Summary of Input Parameters**

1) :math:`k`: diffusion coefficient, k_coh (default 10E-6). Different from the non-cohesive sediment transport, this parameter needs to be specified by users. 

2) :math:`\tau_{cr}`: critical shear stress for erosion, Tau_cr_coh (default 0.001)

3) :math:`\tau_{cd}`: critical shear stress for deposition, Tau_crd_coh (default 0.001)

4) :math:`a,b,m` and :math:`n`: Empirical parameters used to calculate settling velocity, default values are a_coh = 0.1, b_coh=2.0, n_coh=0.5, and m_coh=1.5

5) :math:`E`: erodibility parameter, default E_coh=0.0001

6) :math:`\alpha`: alpha-coefficient used to calculate the erosion rate for soft bed, default alpha_coh = 1.0 


*References*

Kimiaghalam, N., Goharrokhi,M., Clark, S. P., 2016, Estimating cohesive sediment erosion and deposition rates in wide rivers, Canadian Journal of Civil Engineering, 43(2): 164-172 `doi.org/10.1139/cjce-2015-0361 <http://www.nrcresearchpress.com/doi/abs/10.1139/cjce-2015-0361#.XXo-2ZNKjjA>`_

Krone, R. B., 1962, Flume Studies of the Transport of Sediment in Estuarine Shoaling Processes. Final Report to San Francisco District U. S. Army Corps of Engineers, Washington D.C. `website for Krone 1962 <https://www.worldcat.org/title/flume-studies-of-the-transport-of-sediment-in-estuarial-shoaling-processes-final-report/oclc/8967084>`_

Parchure, T. M. and A. J. Mehta, 1985, Erosion of soft cohesive sediment deposits, Journal of Hydraulic Engineering – ASCE 111 no. 10: 1308–1326 `doi/10.1061  <https://ascelibrary.org/doi/10.1061/%28ASCE%290733-9429%281985%29111%3A10%281308%29>`_

Partheniades, E. 1965, Erosion and deposition of cohesive soils, Journal of the hydraulics division. Proceedings of the ASCE 91 no. HY1: 105–139 `website for Partheniades 1965 <https://cedb.asce.org/CEDBsearch/record.jsp?dockey=0013640>`_

Parchure, T. M. and A. J. Mehta, 1985, Erosion of soft cohesive sediment deposits, Journal of Hydraulic Engineering – ASCE 111 no. 10: 1308–1326 `doi:10.1061 <https://ascelibrary.org/doi/10.1061/%28ASCE%290733-9429%281985%29111%3A10%281308%29>`_

Shi, F., J.T. Kirby, J.C. Harris, J.D. Geiman, and S.T. Grilli, 2012, A high-order adaptive time-stepping TVD solver for Boussinesq modeling of breaking waves and coastal inundation. Ocean Modelling, 43-44: 36-51. `DOI: 10.1016/j.ocemod.2011.12.004 <http://www.sciencedirect.com/science/article/pii/S1463500311002010>`_

Soulsby R. L., Hamm L., Klopman, G., Myrhaug, D., Simons R.R., Thomas, G. P., 1993, Wave-current interaction within and outside the bottom boundary layer, Coastal Engineering, Volume 21, Issues 1–3, December 1993, Pages 41-69, `doi:10.1016/0378-3839(93)90045-A <https://doi.org/10.1016/0378-3839(93)90045-A>`_

van Rijn, L.C., 1984, Sediment Pick‐Up Functions, Journal of Hydraulic Engineering
Vol. 110, Issue 10 (October 1984) `doi:10.1061/(ASCE)0733-9429(1984)110:10(1494) <https://doi.org/10.1061/(ASCE)0733-9429(1984)110:10(1494)>`_



