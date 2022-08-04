# Advanced-Physical-Design-Using-OpenLANE-SKY130
</br>![advanced_physical_design](https://user-images.githubusercontent.com/66528639/182838346-13fe85b8-a877-4628-b6aa-13480eea9e3d.png)
</br>Content of Workshop</br>

**</br>Day1 – Inception of open-source EDA, OpenLANE and Sky130 PDK**
</br>How to talk to computers
</br>SoC design and OpenLANE
</br>Starting RISC-V SoC Reference design
</br>Get familiar to open-source EDA tools

**</br>Day 2 - Understand importance of good floorplan vs bad floorplan and introduction to library cells**
</br>Chip Floor planning considerations
</br>Library Binding and Placement
</br>Cell design and characterization flows
</br>General timing characterization parameters

**</br>Day 3 - Design and characterize one library cell using Magic Layout tool and ngspice**
</br>Labs for CMOS inverter ngspice simulations
</br>Inception of Layout – CMOS fabrication process
</br>Sky130 Tech File Labs

**</br>Day 4 - Pre-layout timing analysis and importance of good clock tree**
</br>Timing modelling using delay tables
</br>Timing analysis with ideal clocks using openSTA
</br>Clock tree synthesis TritonCTS and signal integrity
</br>Timing analysis with real clocks using openSTA

**</br>Day 5 - Final steps for RTL2GDS**
</br>Routing and design rule check (DRC)
</br>PNR interactive flow tutorial

**</br>Installation**
</br>Please refer to: https://github.com/nickson-jose/openlane_build_script for installation steps
</br>
**</br>RTL to GDSII Introduction**
</br>From conception to product, the ASIC design flow is an iterative process that is not static for every design. The details of the flow may change depending on ECO’s, IP requirements, DFT insertion, and SDC constraints, however the base concepts still remain. The flow can be broken down into 11 steps:

</br>Architectural Design – A system engineer will provide the VLSI engineer with specifications for the system that are determined through physical constraints. The VLSI engineer will be required to design a circuit that meets these constraints at a microarchitecture modeling level.

</br>RTL Design/Behavioral Modeling – RTL design and behavioral modeling are performed with a hardware description language (HDL). EDA tools will use the HDL to perform mapping of higher-level components to the transistor level needed for physical implementation. HDL modeling is normally performed using either Verilog or VHDL. One of two design methods may be employed while creating the HDL of a microarchitecture:
</br>a. 	RTL Design – Stands for Register Transfer Level. It provides an abstraction of the digital   circuit using:
 
</br>   i. 	Combinational logic
</br>   ii. 	Registers
 </br>  iii. 	Modules (IP’s or Soft Macros)
 
</br>b. 	Behavioral Modeling – Allows the microarchitecture modeling to be performed with behavior-based modeling in HDL. This method bridges the gap between C and HDL allowing HDL design to be performed

</br>RTL Verification - Behavioral verification of design

</br>DFT Insertion - Design-for-Test Circuit Insertion

</br>Logic Synthesis – Logic synthesis uses the RTL netlist to perform HDL technology mapping. The synthesis process is normally performed in two major steps:
  
</br>GTECH Mapping – Consists of mapping the HDL netlist to generic gates what are used to perform logical optimization based on AIGERs and other topologies created from the generic mapped netlist.
</br>Technology Mapping – Consists of mapping the post-optimized GTECH netlist to standard cells described in the PDK
  </br>Standard Cells – Standard cells are fixed height and a multiple of unit size width. This width is an integer multiple of the SITE size or the PR boundary. Each standard cell comes with SPICE, HDL, liberty, layout (detailed and abstract) files used by different tools at different stages in the RTL2GDS flow.

</br>Post-Synthesis STA Analysis: Performs setup analysis on different path groups.

</br>Floorplanning – Goal is to plan the silicon area and create a robust power distribution network (PDN) to power each of the individual components of the synthesized netlist. In addition, macro placement and blockages must be defined before placement occurs to ensure a legalized GDS file. In power planning we create the ring which is connected to the pads which brings power around the edges of the chip. We also include power straps to bring power to the middle of the chip using higher metal layers which reduces IR drop and electro-migration problem.

</br>Placement – Place the standard cells on the floorplane rows, aligned with sites defined in the technology lef file. Placement is done in two steps: Global and Detailed. In Global placement tries to find optimal position for all cells but they may be overlapping and not aligned to rows, detailed placement takes the global placement and legalizes all of the placements trying to adhere to what the global placement wants.

</br>CTS – Clock tree synteshsis is used to create the clock distribution network that is used to deliver the clock to all sequential elements. The main goal is to create a network with minimal skew across the chip. H-trees are a common network topology that is used to achieve this goal.

</br>Routing – Implements the interconnect system between standard cells using the remaining available metal layers after CTS and PDN generation. The routing is performed on routing grids to ensure minimal DRC errors.

</br>The Skywater 130nm PDK uses 6 metal layers to perform CTS, PDN generation, and interconnect routing.
Shown below is an example of a base RTL to GDS flow in ASIC design:
</br>![asic_flow](https://user-images.githubusercontent.com/66528639/182840493-98caa784-61b7-41a0-844d-1fd534614ea2.png)
