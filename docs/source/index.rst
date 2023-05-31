
TripleFrictionPendulumX Element
^^^^^^^^^^^^^^^^

Description
#########

This command is used to construct a TripleFrictionPendulumX element (Kim and Constantinou, 2022,2023a) object, which is an extension of a TripleFrictionPendulum element (Dao et al., 2013). 

The horizontal behavior of the element is achieved by the series model, which consists of properly combined hysteretic/frictional and multidirectional gap elements. 

Two main modifications in the TripleFrictionPendulumX element include that 1) displacement and velocity of the top of the bearing with respect to its bottom computed by the element are partitioned into components at each sliding surface and 2) the factorized friction model used in OpenSees element FPBearingPTV (Kumar et al., 2015) is implemented to account for the effects of pressure, velocity and frictional heating on the friction coefficients at each sliding surfaces calculated by means of the partitioned displacement and velocity. The friction coefficient is given by equation (1) to (4) in which μref is the reference high speed coefficient of friction at the initial time :math:`t = 0`, initial temperature :math:`T_{0} = 20℃` and initial pressure :math:`p_{0}`, :math:`a` is velocity rate parameter :math:`(= 100s/m)`, :math:`p` is the apparent pressure, and :math:`v` is the amplitude of the velocity.

:math:`\mu(p,v,T)=\mu_{ref} k_{p} k_{v} k_{T}`

:math:`k_{p}=(0.7)^{0.02(p-p_{0})}` 

:math:`k_{v}=(1-0.5e^{-av})`

:math:`k_{T}=0.79((0.7)^{0.02T}+0.40)`

In the TripleFrictionPendulumX element, the temperature-dependency of the coefficient of friction was expanded to account for additional cases (Kim and Constantinou, 2023b) beyond the single case described by equation (4) which was implemented in the FPbearingPTV element.  Specifically, two additional cases were added, described by equations (5) and (6), and in Figure 1.  

:math:`k_{T}=0.84((0.7)^{0.0085T}+0.25)`

:math:`k_{T}=0.97((0.7)^{0.029T}+0.22)`


.. figure:: FIGURE 1.tif
   :align: center
   :figclass: align-center
   :width: 700

   Friction Coefficient-Temperature Relationships in TripleFrictionPendulumX element

For more information about the element formulation, please refer to the references at the end of this page.

.. figure:: FIGURE 2.tif
   :align: center
   :figclass: align-center
   :width: 700

   Geometry of Triple FP bearing in accordance with OpenSees Commands
  
Input Parameters
#########
.. admonition:: Command

   **element TripleFrictionPendulumX $eleTag $iNode $jNode $Tag $vertMatTag $rotZMatTag $rotXMatTag $rotYMatTag $kpFactor $kTFactor $kvFactor $Mu1 $Mu2 $Mu3 $L1 $L2 $L3 $d1_star $d2_star $d3_star $b1 $b2 $b3 $W $uy $kvt $minFv $Tol $refPressure1 $refPressure2 $refPressure3 $Diffusivity $Conductivity $Temperature0 $rateParameter $unit $kTmodels**

.. csv-table:: 
   :header: "Argument", "Type", "Description"
   :widths: 5, 5, 40
   
   $eleTag, |integer|, "Unique element object tag"
   $iNode $jNode, |integer| |integer|, "End nodes"
   $Tag, |integer|, "1 for Approach 1 (suitable for all types of analysis), 0 for Approach 2 (1D displacement control analysis only)"
   $vertMatTag, |float|, "Pre-defined material tag for compression behavior of the bearing"
   $rotZMatTag $rotXMatTag $rotYMatTag, |integer| |integer| |integer|, "Pre-defined material tags for rotational behavior about 3-axis, 1-axis and 2-axis, respectively."
   $kpFactor, |integer|, "1.0 if the coefficient of friction is a function of instantaneous axial pressure. :math:`k_{p}=(0.7)^{0.02(p-p_{0})}`"  
   $kTFactor, |integer|, "1.0 if the coefficient of friction is a function of instantaneous temperature at the sliding surface"
   $kvFactor, |integer|, "1.0 if the coefficient of friction is a function of instantaneous velocity at the sliding surface. :math:`k_{v}=(1-0.5e^{-av})`"
   $Mu1 $Mu2 $Mu3, |float| |float| |float|, "Reference friction coefficients, :math:`\mu_i`"
   $L1 $L2 $L3, |float| |float| |float|, "Effective radii of cuvature. :math:`L_i = R_i – h_i`"
   $d1_star $d2_star $d3_star, |float| |float| |float|, "Actual displacement limits of pendulums. :math:`d_i^* = L_i/R_i·d_i`, :math:`d_i` = Nominal displacement capacity of each sliding interface"
   $b1 $b2 $b3, |float| |float| |float|, "Diameters of the rigid slider and the two inner slide plates"
   $W, |float|, "Axial force used for the first trial of the first analysis step"
   $uy, |float|, "Lateral displacement where sliding of the bearing starts. Recommended value = 0.025 to 1 mm. A smaller value may cause convergence problem"
   $kvt, |float|, "Tension stiffness kvt of the bearing"
   $minFv (≥ 0), |float|, "Minimum vertical compression force in the bearing used for computing the horizontal tangent stiffness matrix from the normalized tangent stiffness matrix of the element" 
   $Tol, |float|, "Relative tolerance for checking the convergence of the element. Recommended value = 1.e-10 to 1.e-3"
   $refPressure1 $refPressure2 $refPressure3, |float| |float| |float|, "Reference axial pressures (the bearing pressure under static loads)"
   $Diffusivity, |float|, "Thermal diffusivity of steel (unit: m2/sec) (= 0.444*10-5 for stainless steel)"
   $Conductivity, |float|, "Thermal conductivity of steel (unit: W/m℃) (= 18 for stainless steel)"
   $Temperature0, |float|, "Initial temperature (℃)"
   $rateparameter, |float|, "The exponent that determines the shape of the coefficient of friction vs. sliding velocity curve (unit: sec/m, 100sec/m is used normally)"
   $unit, |integer|, "Tag to identify the unit from the list below. 
   
   1: N, m, s, ℃ 
   
   2: kN, m, s, ℃
   
   3: N, mm, s, ℃
   
   4: kN, mm, s, ℃ 
   
   5: lb, in, s, ℃
   
   6: kip, in, s, ℃
   
   7: lb, ft, s, ℃
   
   8: kip, ft, s, ℃"      
   $kTmodel, |integer|, "Temperature-dependent friction models (3)
   
   1: :math:`k_{T}=0.79((0.7)^{0.02T}+0.40)` (:math:`k_{T}` = 1/2 at 200℃)
   
   2: :math:`k_{T}=0.97((0.7)^{0.029T}+0.22)` (:math:`k_{T}` = 1/3 at 200℃)
   
   3: :math:`k_{T}=0.84((0.7)^{0.0085T}+0.25)` (:math:`k_{T}` = 2/3 at 200℃)"
.. admonition:: Recorders
Recorders
#########
**Typical Element Recorders**

Typical recorders for two-node element are available in the TripleFrictionPendulumX element.

.. csv-table:: 
   :header: "Recorder", "Description"
   :widths: 20, 40
   
   globalForce, global forces
   localForce, local forces
   basicForce, basic forces
   basicDisplacement, basic displacements

**TripleFrictionPendulumX Element Recorders**

Subscripts of the response quantities in the following recorders refer to the numbering of the sliding interfaces, starting from bottom to top sliding interfaces. 

.. csv-table:: 
   :header: "Recorder", "Description"
   :widths: 20, 40
   
   compDisplacement, "Displacements (:math:`u_i`) and velocities (:math:`v_i`) at each sliding surface in the x and y directions :math:`(u_{2x}+u_{3x})/2`, :math:`u_{1x},u_{4x}`,  :math:`(u_{2y}+u_{3y})/2`, :math:`u_{1y}`, :math:`u_{4y}`, :math:`(v_{2x}+v_{3x})/2`, :math:`v_{1x}`, :math:`v_{4x}`,  :math:`(v_{2y}+v_{3y})/2`, :math:`v_{1y}`, :math:`v_{4y}` in accordance with Approach 1 (See Section 3 in Kim and Constantinou, 2022). 
   
   *Example: recorder Element<-file $fileName> -time<-ele ($ele1 $ele2…)>compDisplacement*"
   Parameters, "temperatures (:math:`T_{2,3}`, :math:`T_1`, :math:`T_4`),  friction coefficients (:math:`\mu_{2,3}`, :math:`\mu_1`, :math:`\mu_4`), heat fluxes (:math:`HeatFlux_{2,3}`, :math:`HeatFlux_{1}`, :math:`HeatFlux_4`), pressure dependency factors (:math:`k_{p2,3}`, :math:`k_{p1}`, :math:`k_{p4}`), temperature dependency factors (:math:`k_{T2,3}`, :math:`k_{T1}`, :math:`k_{T4}`), and velocity dependency factors (:math:`k_{v2,3}`, :math:`k_{v1}`, :math:`k_{v4}`).
      
   *Example: recorder Element<-file $fileName> -time<-ele ($ele1 $ele2…)>Parameters*"

Example
#########
.. admonition:: Example
**Tcl Code**
The following codes construct Example 3 in Kim and Constantinou (2023). 

.. code-block:: tcl
# Modeling of Triple FP isolator  			  		    
# Written By: Hyun-myung Kim (hkim59@buffalo.edu)			    
# Date: May, 2023 							    

# Units: N, m, sec
# Remove existing model
wipe

# EXAMPLE 3 (Kim and Constantinou 2023 https://doi.org/10.1002/eqe.3797)
#----------------------------------------------------------------------------
# User Defined Parameters
#----------------------------------------------------------------------------

# TFP Geomoetry of Configuration A 
set L1 0.3937;			# Effective radii of curvature (m)
set L2 3.7465;
set L3 3.7465;
set d1 0.0716;			# Actual displacement capacity (m)
set d2 0.5043;
set d3 0.5043;
set b1 [expr 0.508];  	# Diameter of contact area at the sliding surface (m) 
set b2 [expr 0.711];  
set b3 [expr 0.711];  
set r1 [expr $b1/2];  	# Radius of contact area at the sliding surface (m) 
set r2 [expr $b2/2];  
set r3 [expr $b3/2];  

set uy 0.001; 			# Yield displacement (m)   
set kvc 8000000000.; 	# vertical compression stiffness (N/m)
set kvt 1.; 			# vertical tension stiffness (N/m)
set minFv 0.1; 			# minimum compression force in the bearing (N)

set g 	9.81; 			# gravity acceleration (m/s^2)
set P 	13345e+03; 		# Load on top of TFP 
set Mass [expr $P/$g];  # Mass on top of TFP 
set tol 1.e-5; 			# Relative tolerance for checking convergence

# Heat parameters
set Diffu 0.444e-5;		# Thermal diffusivity (m^2/sec)
set Conduct 18; 		# Thermal conductivity (W/m*Celsius)
set Temperature0 20; 	# Initial temperature (Celsius)

# Friction coefficients (reference)
set mu1 0.01; 
set mu2 0.04;
set mu3 0.08;

# Reference Pressure
set Pref1 [expr $P/($r1*$r1*3.141592)];
set Pref2 [expr $P/($r2*$r2*3.141592)];
set Pref3 [expr $P/($r3*$r3*3.141592)];

#----------------------------------------------------------------------------
# Start of model generation
#----------------------------------------------------------------------------

#Create Model Builder
model basic -ndm 3 -ndf 6

# Create nodes
node 1 0 0 0; # End i
node 2 0 0 0; # End j

# Define single point constraints 
fix 1 	1 1 1 1 1 1;

# Define friction models
set tagTemp 1;
set tagVel 0;
set tagPres 0;
set velRate 100;

#----------------------------------------------------------------------------
# Bring material models and define element
#----------------------------------------------------------------------------

# Creating material for compression and rotation behaviors
uniaxialMaterial Elastic 1 $kvc;
uniaxialMaterial Elastic 2 10.;

set tagT 1; 

# Define TripleFrictionPendulumX element
# element TripleFrictionPendulumX $eleTag $iNode $jNode $tagT $vertMatTag $rotZMatTag $rotXMatTag $rotYMatTag $tagPres $tagTemp $tagVel $mu1 $mu2 $mu3 $L1 $L2 $L3 $d1 $d2 $d3 $b1 $b2 $b3 $W $uy $kvt $minFv $tol $Pref1 $Pref2 $Pref3 $Diffu $Conduct $Temperature0 $velRate $unit $kTmodel
element TripleFrictionPendulumX 1 1 2  $tagT  1 2 2 2 $tagPres $tagTemp $tagVel $mu1 $mu2 $mu3 $L1 $L2 $L3 $d1 $d2 $d3 $b1 $b2 $b3 $P $uy $kvt $minFv $tol $Pref1 $Pref2 $Pref3 $Diffu $Conduct $Temperature0 $velRate 1 1;

#----------------------------------------------------------------------------
# Apply gravity load
#----------------------------------------------------------------------------

#Create a plain load pattern with linear timeseries
pattern Plain 1 "Linear" {
	
	load 2 0. 0. -[expr $P] 0.0 0.0 0.0
}

#----------------------------------------------------------------------------
# Start of analysis generation (Gravity)
#----------------------------------------------------------------------------

system BandSPD
constraints Transformation
numberer RCM
test NormDispIncr 1.0e-15 10 3
algorithm Newton
integrator LoadControl 1
analysis Static

#----------------------------------------------------------------------------
# Analysis (Gravity)
#----------------------------------------------------------------------------

analyze 1
puts "Gravity analysis completed SUCCESSFULLY";

#----------------------------------------------------------------------------
# Start of analysis generation 
# (Sinusoidal; Two cycles of 5s period and 508mm amplitude)
#----------------------------------------------------------------------------

loadConst -time 0.0

#analysis time step 
set dt [expr 0.008]

#excitation time step
set dt1 [expr 0.001] 

timeSeries Trig 11 $dt 10 5 -factor 0.508 -shift 0

pattern MultiSupport 2 {
groundMotion 1 Plain -disp 11 
# Node, direction, GMtag
imposedMotion 2 2 1
}

#----------------------------------------------------------------------------
# Start of recorder generation (Sinusoidal)
#----------------------------------------------------------------------------

# Set up recorder
set OutDir 		EXAMPLE3;			# Output folder
set OutFile1	TEMPERATURE.txt;
set OutFile2 	DISP.txt; 		  
set OutFile3	FORCE.txt;
set OutFile4	COMPDISP.txt;

file mkdir $OutDir;
recorder Element -file $OutDir/$OutFile1 -time -ele 1 Parameters;
recorder Node -file $OutDir/$OutFile2 -time -nodes 2 -dof 1 2 3 disp;
recorder Element -file $OutDir/$OutFile3 -time -ele 1 basicForce;
recorder Element -file $OutDir/$OutFile4 -time -ele 1 compDisplacement;

#----------------------------------------------------------------------------
# Analysis (Sinusoidal)
#----------------------------------------------------------------------------

system SparseGeneral
constraints Transformation
test NormDispIncr 1.0e-5 20 0
algorithm Newton
numberer Plain
integrator Newmark 0.5 0.25
analysis Transient

# set some variables
set tFinal [expr 10]
set tCurrent [getTime]
set ok 0

# Perform the transient analysis
while {$ok == 0 && $tCurrent < $tFinal} {
    
    set ok [analyze 1 $dt]
    
    # if the analysis fails try initial tangent iteration
    if {$ok != 0} {
	puts "regular newton failed .. lets try an initail stiffness for this step"
	test NormDispIncr 1.0e-12  100 0
	algorithm ModifiedNewton -initial
	set ok [analyze 1 $dt]
	if {$ok == 0} {puts "that worked .. back to regular newton"}
	test NormDispIncr 1.0e-12  10 
	algorithm Newton
    }
    
    set tCurrent [getTime]
}

# Print a message to indicate if analysis succesfull or not
if {$ok == 0} {
   puts "Transient analysis completed SUCCESSFULLY";
} else {
   puts "Transient analysis completed FAILED";    
}

.. admonition:: References 

   #. Dao, N. D., Ryan, K. L., Sato, E. and Sasaki, T. (2013). “Predicting the displacement of triple pendulum bearings in a full-scale shaking experiment using a three-dimensional element”, Earthquake engineering and structural dynamics, 42(11), 1677-1695. doi.org/10.1002/eqe.2293
	
   #. Kim, H-M., and Constantinou, M. C. (2022). “Modeling triple friction pendulum bearings in program OpenSees including frictional heating effects”, Report No. MCEER-22-0001, Multidisciplinary Center for Earthquake Engineering Research, Buffalo, NY.
	
   #. Kim, H-M., and Constantinou, M. C. (2023a). “Modeling frictional heating effects in triple friction pendulum isolators”, Earthquake Engineering & Structural Dynamics. doi.org/10.1002/eqe.3797
	
   #. Kim, H-M., and Constantinou, M. C. (2023b). “Development of Performance-based Testing Specifications for Seismic Isolators”, Report No. MCEER-23-xxxx, Multidisciplinary Center for Earthquake Engineering Research, Buffalo, NY.
   
   #. Kumar, M., Whittaker, A. S., and Constantinou, M. C. (2015). “Characterizing friction in sliding isolation bearings”, Earthquake Engineering & Structural Dynamics, 44(9), 1409-1425. doi.org/10.1002/eqe.2524
   
Code Developed by: **Hyun-myung Kim** and **Michael C. Constantinou**, University at Buffalo, NY
