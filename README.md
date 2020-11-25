# adv_PhysicalDesign_Wrkshp


# Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK
## IC Design Terminologies:
Integrated Chip (IC) is basically an integration of large number of transistors, resistors, capacitors fabricated into a single unit, performing a specific function. Based on the functions they perform, the ICs are categoried to analog and digital ICs. These ICs are enclosed in **Packages**, in the market there are multiple variation of Packages available such as through-hole mount packages, pin-grid array packages etc. The one package introduced in this workshop is QFN-48 (Quad Flat NO Leads) with a size of 7mmx7mm, here, 48 are the total number of pins that supports the information exchanges to and from the IC to the other parts of the design, variable pin packages are also available. 
Chip, the chip generally sits at the centre of the package and are connected to the pins of the package through wire-bonds. The Chip has various components some of them are:
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
## Introduction to OpenLANE
On the availability of open-sourced PDK, an open-sourced methodology and flow is introduced by **efabless** and named it as OpenLANE. It has two modes of operation:
* Autonomous : it performs all of the ASIC flow in one step
* Interactive : It is a step-by-step process to perform every phase of ASIC flow with specific commands.

## OpenLANE detailed ASIC Design Flow:



# Day 1: Lab Instance
## OpenLane Directory & Invoking OpenLane
pdk directory exploration: We are using skywater-130 pdk. 
skywater pdk --> which has all pdk files.. tech lib, lef.lib
open_pdks: all foundry files they are compatible with commercial eda tools not for open eda tools. this open_pdk directory overcomes this issue. It basically contains scripts.
sky130A: the pdk thats made compatible to open source environment
we will work on sky130_fd_sc_hd pdk variant

**Invoking Openlane** :
* ./flow.tcl -interactive  --> script to run openlane and interactive switch tells the tool will run in step-by-step mode default is autonomous
* import all packages to run the flow : packages requires openlane 0.9
* prep stage: sets up all files for our design,. prep -design picorv32a
* run_synthesis

