# Advanced-Physical-Design-Using-OpenLANE-SKY130
</br>![advanced_physical_design](https://user-images.githubusercontent.com/66528639/182838346-13fe85b8-a877-4628-b6aa-13480eea9e3d.png)

**</br>Content of Workshop</br>**
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

**</br>1.Architectural Design** – A system engineer will provide the VLSI engineer with specifications for the system that are determined through physical constraints. The VLSI engineer will be required to design a circuit that meets these constraints at a microarchitecture modeling level.

**</br>2.RTL Design/Behavioral Modeling** – RTL design and behavioral modeling are performed with a hardware description language (HDL). EDA tools will use the HDL to perform mapping of higher-level components to the transistor level needed for physical implementation. HDL modeling is normally performed using either Verilog or VHDL. One of two design methods may be employed while creating the HDL of a microarchitecture:
**</br>a. 	RTL Design** – Stands for Register Transfer Level. It provides an abstraction of the digital   circuit using:
 
 **</br>   i.Combinational logic**
 **</br>   ii.Registers**
   **</br> iii.Modules (IP’s or Soft Macros)**
 
**</br>b. 	Behavioral Modeling** – Allows the microarchitecture modeling to be performed with behavior-based modeling in HDL. This method bridges the gap between C and HDL allowing HDL design to be performed

**</br>3.RTL Verification** - Behavioral verification of design

**</br>4.DFT Insertion** - Design-for-Test Circuit Insertion

**</br>5.Logic Synthesis** – Logic synthesis uses the RTL netlist to perform HDL technology mapping. The synthesis process is normally performed in two major steps:
  
**</br>6.GTECH Mapping** – Consists of mapping the HDL netlist to generic gates what are used to perform logical optimization based on AIGERs and other topologies created from the generic mapped netlist.

**</br>7.Technology Mapping** – Consists of mapping the post-optimized GTECH netlist to standard cells described in the PDK
  </br>Standard Cells – Standard cells are fixed height and a multiple of unit size width. This width is an integer multiple of the SITE size or the PR boundary. Each standard cell comes with SPICE, HDL, liberty, layout (detailed and abstract) files used by different tools at different stages in the RTL2GDS flow.

**</br>8.Post-Synthesis STA Analysis**: Performs setup analysis on different path groups.

**</br>9.Floorplanning** – Goal is to plan the silicon area and create a robust power distribution network (PDN) to power each of the individual components of the synthesized netlist. In addition, macro placement and blockages must be defined before placement occurs to ensure a legalized GDS file. In power planning we create the ring which is connected to the pads which brings power around the edges of the chip. We also include power straps to bring power to the middle of the chip using higher metal layers which reduces IR drop and electro-migration problem.

**</br>10.Placement** – Place the standard cells on the floorplane rows, aligned with sites defined in the technology lef file. Placement is done in two steps: Global and Detailed. In Global placement tries to find optimal position for all cells but they may be overlapping and not aligned to rows, detailed placement takes the global placement and legalizes all of the placements trying to adhere to what the global placement wants.

**</br>11.CTS** – Clock tree synteshsis is used to create the clock distribution network that is used to deliver the clock to all sequential elements. The main goal is to create a network with minimal skew across the chip. H-trees are a common network topology that is used to achieve this goal.

**</br>12.Routing** – Implements the interconnect system between standard cells using the remaining available metal layers after CTS and PDN generation. The routing is performed on routing grids to ensure minimal DRC errors.

</br>The Skywater 130nm PDK uses 6 metal layers to perform CTS, PDN generation, and interconnect routing.
Shown below is an example of a base RTL to GDS flow in ASIC design:
</br>![asic_flow](https://user-images.githubusercontent.com/66528639/182840493-98caa784-61b7-41a0-844d-1fd534614ea2.png)

**</br>ASIC design flow**
</br>Process Design Kit (PDK) is the interface between the CAD designers and the foundry. The PDK is a collection of files used to model a fabrication process for the EDA tools used in designing an IC. PDK’s are traditionally closed-source and hence are the limiting factor to open-source Digital ASIC Design. Google and Skywater have broken this stigma and published the world’s first open-source PDK on June 30th, 2020. This breakthrough has been a catalyst for open-source EDA tools. This workshop focuses on using the open-source RTL2GDS EDA tool, OpenLANE, in conjunction with the Skywater 130nm PDK to perform the full RTL2GDS flow as shown below:
</br>![openlane_flow](https://user-images.githubusercontent.com/66528639/182841773-c66eb755-3c89-4441-ab66-09ca7468174d.png)
</br>OpenLANE flow consists of several stages. By default, all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLANE can also be run interactively as shown here.

**</br>Synthesis**
      </br> Yosys - Performs RTL synthesis using GTech mapping
      </br> abc - Performs technology mappin to standard cells described in the PDK. We can adjust synthesis techniques using    different integrated abc scripts to get desired results
     </br> OpenSTA - Performs static timing analysis on the resulting netlist to generate timing reports
     </br> Fault – Scan-chain insertion used for testing post fabrication. Supports ATPG and test patterns compaction
  
**</br>Floorplan and PDN**

  
     Init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
     Ioplacer - Places the macro input and output ports
     PDN - Generates the power distribution network
     Tapcell - Inserts welltap and decap cells in the floorplan
     Placement – Placement is done in two steps, one with global placement in which we place the designs across the chip, but they will not be legal placement with some standard cells overlapping each other, to fix this we perform a detailed placement which legalizes the design and ensures they fit in the standard cell rows
     RePLace - Performs global placement
     Resizer - Performs optional optimizations on the design
     OpenPhySyn - Performs timing optimizations on the design
     OpenDP - Perfroms detailed placement to legalize the globally placed components
  
  **</br>CTS**
  
     TritonCTS - Synthesizes the clock distribution network
  
**</br>Routing**

        FastRoute - Performs global routing to generate a guide file for the detailed router
      
      TritonRoute - Performs detailed routing from global routing guides
     SPEF-Extractor - Performs SPEF extraction that include parasitic information
  
**</br>GDSII Generation**

        Magic - Streams out the final GDSII layout file from the routed def
  
**</br>Checks**
  
     Magic - Performs DRC Checks & Antenna Checks
     Netgen - Performs LVS Checks
     
**</br>Day1 – Inception of open-source EDA, OpenLANE and Sky130 PDK**
</br>
**</br>OpenLANE flow</br>**
</br>
</br>![1](https://user-images.githubusercontent.com/66528639/182842765-7dfbc07a-5819-434d-85f8-868704b91eae.jpg)
</br>![2](https://user-images.githubusercontent.com/66528639/182842821-321d4145-11f9-4321-96b8-c5775019d751.jpg)
</br>![3](https://user-images.githubusercontent.com/66528639/182843164-56bbba60-bc22-4a97-8601-af9affcc78c6.jpg)
</br>![4](https://user-images.githubusercontent.com/66528639/182843185-2069deec-f812-412e-8a70-d2fdcf77483f.jpg)
</br>![5](https://user-images.githubusercontent.com/66528639/182843262-f3074c08-ef57-40c7-98cd-bdba41632bdb.jpg)
</br>![6](https://user-images.githubusercontent.com/66528639/182843333-3a4e5249-d895-4d0d-bb97-dccd11d6d927.jpg)
</br>![7](https://user-images.githubusercontent.com/66528639/182843399-56a417aa-a0e7-4be8-a668-b30ddd83577f.jpg)
</br>![8](https://user-images.githubusercontent.com/66528639/182843414-b5df2bf6-62fe-492f-8db1-1cca8cc698df.jpg)
</br>![9](https://user-images.githubusercontent.com/66528639/182843433-34c06e6b-aa81-4963-8a7e-fc62847a3dfa.jpg)
</br>![10](https://user-images.githubusercontent.com/66528639/182843453-5e99b87c-cb40-4660-9cfa-313a942082b6.jpg)
</br>![11](https://user-images.githubusercontent.com/66528639/182843523-7d16ccb5-4dd1-45ad-a2f4-a3d42cf20bd3.jpg)
</br>![12](https://user-images.githubusercontent.com/66528639/182843548-f57abf23-7f25-40fa-93e7-341a427e499c.jpg)
</br>![13](https://user-images.githubusercontent.com/66528639/182843581-232aa97b-6731-4a17-be43-7c0e04ce09da.jpg)
</br>![14](https://user-images.githubusercontent.com/66528639/182843598-c6df5d6a-b738-4d4d-90cf-985c14d36803.jpg)
</br>![15](https://user-images.githubusercontent.com/66528639/182843665-4d192f2d-239d-4a7c-b2b2-39ed91578bbd.jpg)
</br>![16](https://user-images.githubusercontent.com/66528639/182843707-586317c6-7adf-42a7-973b-69efe476522e.jpg)
</br>![17](https://user-images.githubusercontent.com/66528639/182843743-23420c0a-5f79-49ec-9ece-5824386b231a.jpg)
</br>![18](https://user-images.githubusercontent.com/66528639/182843762-a62ea223-c037-4c0d-b5ec-4f13d72504d7.jpg)
</br>![19](https://user-images.githubusercontent.com/66528639/182843777-6d6ad852-1c9b-4ddf-ba0e-ca8d3904cc35.jpg)
</br>
</br>![20_inovking_directory_openlane](https://user-images.githubusercontent.com/66528639/182843850-0801ec7a-ce9d-4a54-844a-c1b7ce1abf0c.jpg)
**</br>Directory OpenLANE</br>**

</br>![21_inovking_openlane](https://user-images.githubusercontent.com/66528639/182843953-90ba34fb-045c-4b94-8061-e4c00edc58c4.jpg)
**</br>Invoking OpenLANE</br>**

</br>![22_setting_environment_design](https://user-images.githubusercontent.com/66528639/182844100-7f3414d9-6dcb-4c5c-9a83-ed0e1a195218.jpg)
**</br>Setting Environment for Design</br>**

</br>![23_synthesis_report](https://user-images.githubusercontent.com/66528639/182844170-921b0458-92d9-4743-9b91-ef5cee7ec494.jpg)</br>
**</br>Synthesis Report of PICORV32a</br>**

**</br>Day 2 - Understand importance of good floorplan vs bad floorplan and introduction to library cells**
</br>
</br>![24_floorplan_magic](https://user-images.githubusercontent.com/66528639/182997205-05816076-6db2-4026-b2ea-e202157ce148.jpg)
**</br>Floorplan in OpenLANE</br>**

</br>![25_placement](https://user-images.githubusercontent.com/66528639/182997218-993a68a3-6ec2-4eba-9980-65adefd12646.jpg)
**</br>Placement in OpenLANE</br>**

**</br>Day 3 - Design and characterize one library cell using Magic Layout tool and ngspice**

</br>![1 Magic_Tech_file_path](https://user-images.githubusercontent.com/66528639/183261353-49f90892-c77f-4c33-8429-fa3cdf25dc31.jpg)
</br>Magic Tech File Path

</br>![2 Inverter_layout](https://user-images.githubusercontent.com/66528639/183261357-7026af22-6828-406a-bd11-8fc4ed816827.jpg)
</br>Inverter Layout view in Magaic T

</br>![3 Prasitics_Extraction](https://user-images.githubusercontent.com/66528639/183261363-eee58782-856b-4d87-a250-615a42608c54.jpg)
</br>Prasitic Extraction of Inverter

</br>![4 Extracted_spice_file](https://user-images.githubusercontent.com/66528639/183261386-d20ad05e-2a30-4eb7-84f0-3fbf3e64b84c.jpg)
</br>Extration of Spice File

</br>![5 Spice_file](https://user-images.githubusercontent.com/66528639/183261393-2abed6b1-472e-45b2-9bab-27c557952c47.jpg)
</br>Spice File

</br>![6 ngspice_run](https://user-images.githubusercontent.com/66528639/183261396-a5611eee-0ebd-4038-b641-89d2b1e1f56c.jpg)
</br>ngspice run

**</br>Day 4 - Pre-layout timing analysis and importance of good clock tree**
</br>![Uploading 1.Grid_spacing.jpg…]()
</br>Grid Spacing
</br>![2 lef_generation](https://user-images.githubusercontent.com/66528639/183261515-33031b04-62fb-4de3-a18a-364891bddcd6.jpg)
</br>Lef Generation from Layout
</br>![3 New_config_file](https://user-images.githubusercontent.com/66528639/183261518-6886ba8d-9920-4aa5-93da-59231375b2ef.jpg)
</br>New Configuration file
</br>![4 synthesis_new_config_file](https://user-images.githubusercontent.com/66528639/183261525-dc74a934-50cc-42a3-a04b-fb11725914bc.jpg)
</br>Synthesis report for new configuration file
</br>![5 Lef file](https://user-images.githubusercontent.com/66528639/183261877-98e73a0d-dda0-437f-81c6-2ec3f0b3888d.jpg)
</br>Lef file of inverter

**</br>Config.tcl**
</br>
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "12.000"
set ::env(CLOCK_PORT) "clk"


set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd_fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]


set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}

</br>![6 final_routing_1](https://user-images.githubusercontent.com/66528639/183262184-6a14f414-5b6f-46b3-89c5-98d7a1762ca5.jpg)

</br>Final inverter in picorv32a

</br>



















































