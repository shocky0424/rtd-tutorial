
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

   The following code constructs Example 3 in Kim and Constantinou (2023). 

   1. **Tcl Code**

   .. code-block:: tcl

      # --------------------------------------------------------------------------------------------------
      # 3D Steel L-section beam subjected to compressive load on shear center
      # Xinlong Du, 9/25/2019
      # Displacement-based beam-column element for asymmetric sections
      # --------------------------------------------------------------------------------------------------
      set systemTime [clock seconds] 
      puts "Starting Analysis: [clock format $systemTime -format "%d-%b-%Y %H:%M:%S"]"
      set startTime [clock clicks -milliseconds];
      # SET UP ----------------------------------------------------------------------------
      wipe;				# clear memory of all past model definitions
      model BasicBuilder -ndm 3 -ndf 6;	# Define the model builder, ndm=#dimension, ndf=#dofs
      set dataDir Data;			# set up name of data directory
      file mkdir $dataDir; 			# create data directory
      source LibUnits.tcl;			# define units
      source DisplayPlane.tcl;		# procedure for displaying a plane in model
      source DisplayModel3D.tcl;		# procedure for displaying 3D perspectives of model
      # define GEOMETRY ------------------------------------------------------------------
      #Nodes, NodeNumber, xCoord, yCoord, zCoord
      for {set i 1} {$i<8} {incr i 1} {
	      node $i [expr -9.2+9.2*$i] 0 0;
      }
      # ------ define boundary conditions
      # NodeID,dispX,dispY,dispZ,rotX,RotY,RotZ 
      fix 1  1 1 1 1 1 1;    
      set StartNode 1;
      set EndNode 7;
      # Define  SECTIONS -------------------------------------------------------------
      set ColSecTag 1
      # define MATERIAL properties 
      set Es [expr 27910.0*$ksi];		# Steel Young's Modulus
      set nu 0.3;
      set Gs [expr $Es/2./[expr 1+$nu]];  # Torsional stiffness Modulus
      set matID 1
      uniaxialMaterial Elastic $matID $Es;
      set J [expr  0.02473958*$in4]
      set GJ [expr $Gs*$J]
      set z0 [expr 0.64625474*$in];
      set y0 [expr -0.68720012*$in];
      source L3x2x0_25.tcl;
      # define ELEMENTS-----------------------------------------------------------------------------------------------
      set IDColTransf 1; # all members
      set ColTransfType Corotational;		# options for columns: Linear PDelta Corotational 
      geomTransf $ColTransfType  $IDColTransf 0 0 1;	#define geometric transformation: performs a corotational geometric transformation
      set numIntgrPts 2;	# number of Gauss integration points
      for {set i 1} {$i<$EndNode} {incr i 1} {
      set elemID $i
      set nodeI $i
      set nodeJ [expr $i+1]
      element dispBeamColumnAsym $elemID $nodeI $nodeJ $numIntgrPts $ColSecTag $IDColTransf -shearCenter $y0 $z0;	
      } 

      # Define RECORDERS -------------------------------------------------------------
      recorder Node -file $dataDir/DispDB6.out -time -node $EndNode -dof 1 2 3 4 5 6 disp;			# displacements of middle node
      recorder Node -file $dataDir/ReacDB6.out -time -node $StartNode -dof 1 2 3 4 5 6 reaction;		# support reaction

      # Define DISPLAY -------------------------------------------------------------
      DisplayModel3D DeformedShape;	 # options: DeformedShape NodeNumbers ModeShape

      # define Load------------------------------------------------------------- 
      set N 15.0;
      pattern Plain 2 Linear {
        # NodeID, Fx, Fy, Fz, Mx, My, Mz
        load $EndNode -$N 0 0 0 0 0; 
      }

      # define ANALYSIS PARAMETERS------------------------------------------------------------------------------------
      constraints Plain; # how it handles boundary conditions
      numberer Plain;	   # renumber dof's to minimize band-width 
      system BandGeneral;# how to store and solve the system of equations in the analysis
      test NormDispIncr 1.0e-08 1000; # determine if convergence has been achieved at the end of an iteration step
      #algorithm NewtonLineSearch;# use Newton's solution algorithm: updates tangent stiffness at every iteration
      algorithm Newton;
      set Dincr -0.01;
                                     #Node,  dof, 1st incr, Jd, min,   max
      integrator DisplacementControl $EndNode 1   $Dincr     1  $Dincr -0.01;
      analysis Static	;# define type of analysis static or transient
      analyze 7000;
      puts "Finished"
      #--------------------------------------------------------------------------------
      set finishTime [clock clicks -milliseconds];
      puts "Time taken: [expr ($finishTime-$startTime)/1000] sec"
      set systemTime [clock seconds] 
      puts "Finished Analysis: [clock format $systemTime -format "%d-%b-%Y %H:%M:%S"]"


