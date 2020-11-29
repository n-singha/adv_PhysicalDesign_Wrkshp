# adv_PhysicalDesign_Wrkshp
A workshop offering hands-on experience with OpenLANE EDA tool using the PDK of Skywater 130nm technology (more breifing at the end of day 5). 


# Day 1: Introduction to IC design terminologies, OpenLANE and Sky130 PDK
## IC Design Terminologies:
Integrated Chip (IC) is basically an integration of large number of transistors, resistors, capacitors fabricated into a single unit, performing a specific function. Based on the functions they perform, the ICs are categoried to analog and digital ICs. 

These ICs are enclosed in **Packages**, in the market there are multiple variation of Packages available such as through-hole mount packages, pin-grid array packages etc. The one package introduced in this workshop is QFN-48 (Quad Flat NO Leads) with a size of 7mmx7mm, here, 48 are the total number of pins that supports the information exchanges to and from the IC to the other parts of the design, variable pin packages are also available. 

**Chip** : the chip generally sits at the centre of the package and are connected to the pins of the package through wire-bonds. The Chip has various components some of them are:
  * Pads: they send information inside the chip and vice versa.
  * Core: all the logic or functionality of the chip are placed inside the core
  * Die: Area of the entire chip

**Foundry IPs and Macros**
Any typical core consists of SOC, SRAM, ADC, PLL and some other components. The PLL, SRAM, ADC, DAC in the chip core falls under the category of Foundry IPs. Foundry is the place where the chips get manufactured. In order to make IPs, some amount of intelligence is needed. Marocs are based on pure digital logic. 

## RISC-V:
RISC-V is an open standard instruction set architecture (ISA) based on established reduced instruction set computer (RISC) principles. Unlike most other ISA designs, the RISC-V ISA is provided under open source licenses that do not require fees to use. The base RISC-V is a 32-bit processor architecture with 31 general-purpose registers. All instructions are 32 bits long. Here the RISC-V implementation is picorv32. Risc-v is the specification and picorv32 is the rtl implementation of the spec. and from there it is the normal rtl to GDS flow.

## Components of ASIC flow digital design
For designing Open-Source Application Specific Integrated Circuits (ASIC) in automated way certain set of components are necessary and these components have to be opensourced i.e. open for the masses, they are: 
* RTL ips: there are many open-source rtl-ip available
* EDA tools: open-source tools --> openroad, openflow, qflow
* PDK Data:  PDK stands for Process Design Kits,it is an interface between FAB and designers and contains information such as DRC and LVS rules, SPICE Model Parameters, Digital standard cell libraries etc. Earlier PDKs are under non-disclosure agreement,now, Google + Skywater tech, open-sourced the PDKs of 130nm tech on june 30,2020. 

## RTL2GDS Flow

![](day1/ASIC_flow.PNG)

## Introduction to OpenLANE
On the availability of open-sourced PDK, an open-sourced methodology and flow is introduced by ***efabless*** and named it as **OpenLANE**. It is an automated RTL2GDS flow tool that is built around various open-source EDA tools such as yosys, openROAD, Magic etc. It has two modes of operation:
* Autonomous : it performs all of the ASIC flow in one step
* Interactive : It is a step-by-step process to perform every phase of ASIC flow with specific commands.

## OpenLANE detailed ASIC Design Flow:

![](day1/openlane_flowchart.PNG)


# Day 1: Lab Instance
## PDK directory exploration:
We are using skywater-130 pdk. In the terminal, a pdk directory is provided which contains all pdk related informations
* skywater pdk --> which has all pdk files.. tech lib, lef.lib
* open_pdks: all foundry files they are compatible with commercial eda tools not for open eda tools. This open_pdk directory overcomes this issue. It basically contains scripts.
* sky130A: the pdk thats made compatible to open source environment
   * libs.ref: the contents of this file are technology related. The skywater 130nm technology and sky130_fd_sc_hd pdk variant is used for the physical design flow in this workshop
   * libs.tech: Tool related files are the content. 
   
   This snapshot shows the various files under the pdk directory.
   
   ![](day1/pdk_explore.PNG )

## Invoking Openlane: 

Commands used: 
```
 ./flow.tcl -interactive   //script to run openlane; interactive switch tells the tool will run in step-by-step mode; default is autonomous
 packages requires openlane 0.9 //import all packages to run the flow 
```

![](day1/open_pack.PNG)

## Prepration Stage:

Command used: 
```
prep -design picorv32a
```

* the openlane has many designs incoporated in it the switch ***-design*** followed by *<design_name>* points to the design we will use in our PD flow.
* ***-tag*** this switch followed by *<custom_name>* creates a custom name in the run folder of the design.
* ***-overwrite*** this switch erases everything created under the run folder
* the command *set* helps chnaging any value in the config file within the Openlane environment itself. 

Command format using switches:
```
prep -design <design_name> -tag <custom_name_for run> -overwrite
```

![](day1/prep.PNG)

Snapshot showing changes done in the openlane environment, the use of the *set* command for changing the value of clock on the openlane environment. 

![](day1/chnges_oplne.PNG )

## Synthesis stage: 

Command used:
```
run_synthesis
```

![](day1/run_synth.PNG)

The below image shows the statistics in synthesis report from where we can calculate buffer or flop ratio.  
![](day1/ratio_calc.PNG)

The area shown in the synthesis report.

![](day1/area_chip.PNG)

# Day 2: Floorplan considerations and introduction to standard cell flow
## Floorplan consideration
The following aspects are considered in the floorplaning aspects:
* core and die area
* preplaced cell
* Power planing
* Pin placement 
* Cell blockage

## Library Binding and Placement


# Day 2: Lab instance

## Runing Floorplan:
After preparation of the design, runing synthesis the design is now ready for the floorplan phase of the physical design flow.
command used:
```
run_floorplan
```

Openlane provides various switches that can direct the floorplan. The pnr flow is an iterative flow, the switches can be set or unset based on the what iterative part we are in.
A snapshot of the switches is shown:

![](day2_lab/floorplan_switch.PNG)

Metal layer defaults in the floorplan.tcl file:

![](day2_lab/floorplan_tcl_defaults.PNG)

IO_Log file generated after running floorplan:

![](day2_lab/io_logfile_floorplan.PNG)

Floorplan Def file: 
The floorplan plan def gives us the die area of the chip in the format ***(llx,lly) (urx,ury)***

![](day2_lab/die_area.PNG)


Reviewing the layout after the floorplan:

command used:
```
magic -T <tech_file> lef read <lef file> def read <floorplan.def>
```
![](day2_lab/metal_layer_pins.PNG)

## Placement run:
In the placement phase, the standard cells are placed in the floorplan recieved from the floorplan phase. The placement, in the initial stage of flow is constrained on congestion part and further down the PnR flow, the placement is made timing constraint.

Command used to run: 

```
run_placement
```
![](day2_lab/placement_run.PNG )

The layout review after placement in Magic:

![](day2_lab/placement_layout.PNG)


# Day 3: Standard cell characterization and introduction to sky130 tech files
# Day 3: Lab 
In Day 3 lab, we used a custom inverter cell, we cloned this standard cell from *https://github.com/nickson-jose/vsdstdcelldesign* and used the layout of this cell to run the post ngspice simulations and calculate the characterization parameters of the inverter cell. This cell will then be plugged into the picorv32 design, in the subsequent days.

### Command used to see the inverter layout in magic: 

```
magic -T sky130A.tech sky130_inv.mag  // this command is ran in the cloned directory else path to tech file needs to be provided.
```
* cloning the *stdcelldesign*:

![](day3_lab/git_clone.PNG)


* Inverter Layout

![](day3_lab/inv_cell.PNG)

* identifying transistor in the layout

![](day3_lab/pmos_nmos_identify.PNG)

* extracting the spice files
  * commands to extract spice file
  ```
  extract all
  ext2spice cthresh 0 rthresh 0  // using cthresh, rthresh extract only those that are above 0 F or ohm. These are Filter threshold
  ext2spice
  ```

![](day3_lab/spice_extraction.PNG)


### Spice Commands to be included in the extracted spice file:
* mos transitors inclusion: 
```
include ./libs/nshort.lib
include ./libs/nshort.lib
```
* description of netlist
```
M1 out in  VPWR VPWR pshort w=37 l=23
M2 out in  VGND VGND nshort w=35 l=23
```
* simulation command used
```
.tran 1n 10n
```
command to open ngspice env:
```
ngspice spice_inv.spice
```


**Characterization Parameters:**

Cell delay: Difference between 50% output transition and 50% input transition.

Rise delay: Time taken for waveform to rise from 20% to 80% of VDD.

Fall delay: Time taken for waveform to fall from 80% to 20% of VDD.


Spice simulation: 

![](day3_lab/cell_characterization.PNG)

Skywater 130 tech file: 
![](day3_lab/tech_file.PNG)


# Timing Analysis and Introduction to CTS and Triton CTS
# Day 4: Lab
In Day 3, we characterized the standard cell, on Day 4 lab we will plug this standard cell into our picorv32a design and then perform timing analysis and CTS. 


## Plugging the stdcell into picorv32a:
Steps:
* extract the *lef* file










