# VSD-IAT openLANE workshop

![1](https://user-images.githubusercontent.com/63381455/106157595-e2a81b80-61a8-11eb-8880-0fdebcb7cb2c.JPG)
 
# Table of contents

- [Overview](#overview)
- [Pre requisite](#Prerequisite)
- [Introduction](#Introduction)'
- [Day 1 ](#Day1)
  - [Introduction to QFN -48 package, chip, pads, core, die, IPs](#Imtro)
  - [Introduction to RISCV](#RiscV)
  - [OpenLANE design ASIC flow](#soc)
  - [Part -2 Get familiar with EDA tools](#familiar)   
  - [Invoking openLANE](#invoking)
  - [Import package](#import)
  -[Prepare design](#prepare)
- [Day 2](#Day2)
  - [Floorplan](#Floorplan)
  - [Cell core and die area](#Cell core and die area)
  - [Aspect ratio and utilization factor](#Aspect ratiod and utilization factor)
  - [Concept of pre placed cells](#Concept of pre placed cells)
  - [Decoupling capacitors](#Decoupling capacitors)
  - [Power planning](#Power planning)
  - [Pin placement](#Pin placement)



# Overview

This repository is about the workshop conducted by VSD-IAT on advance physical design using the emerging trend of open source EDA tool openLANE and open source pdk sky130. In this workshop VSD provides overall training through interactive tutorials on the fundamentals of physical design as well as in hand experience on the use of the openLANE/sky130 flow . OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII .https://github.com/efabless/openlane.

# Pre requisite
 As for as the workshop is concerned the only requirement is the registered email id. VSD-IAT is an expertly build cloud training platform with zero restriction on using the platform across all time zones. The workshop design method provides freedom for use of the open source tools on new designs after the workshop https://www.vlsisystemdesign.com/vsd-iat/. 
 
However, for local use of the openLANE tool a set of instructions are to be followed. The initial requirement is an Ubuntu based system with at least 25GB+disk space. This repository https://github.com/nickson-jose/openlane_build_script gives the detailed instruction for building openLANE into the local system.
 
 # Introduction
 
Verilog RTL is basically a design written in Verilog Language which is a high level language. Either written in behavioral or structural form famously.
GDSII is the database file format which industry follows as a standard format for exchange of data related to IC layout. It is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form. The workshop starts with the self assessement process via some rapid fire question which assists in determining the understanding of the process follwed by the tutorials to learn the concepts. The main focus of this workshop is to use openLANE inconjuction with Google SKywater 130nm for full open source ASIC design.
 

# Day 1 - Part 1

### Introduction to QFN -48 package, chip, pads, core, die, IPs

Integrated circuits or IC are devices where in number of componenets such as transistors, diodes, resistors capacitors etc are amalgameted /interconnected to a single semiconductor chip. A package/chip are small in size and can broadly be classified into surface mount type and through hole type. Some of the surface mount and through type ic are as shown in the figure below and is sourced from https://components101.com/articles/different-ic-package-types-and-which-one-should-you-select.

  ![pac](https://user-images.githubusercontent.com/63381455/106300931-1785a280-627d-11eb-92ef-fac5e89eb5df.JPG)
  
  In this workshop a quad flat no leads/QFN -48 package is used for explanation. It is a 7mmX7mm package consisting of foundry IPs and macros. The chip sits at the center and its boundary is connected to pins of the package by wire bond which assits in transferring signal from outside of the chip.
  
  ![2-4](https://user-images.githubusercontent.com/63381455/106305674-faec6900-6282-11eb-8561-5c5901bdd013.JPG)
  
 A typical chip is as shown in the figure  below which consist of SOC (RISCV), ADC, DAC, memory, SPI and other components. The components can be classified into macros, foundry IPS. Macros are pure digital logic and IP's are known as intellectual property which requires some amount of intelligence to be built. The PADS forms the connection between inside and outside the chip. The core chip is the area where the digital logic sits and die basically is the size of the chip which is fabricated into the silicon wafer. 

![6-8](https://user-images.githubusercontent.com/63381455/106305390-a6e18480-6282-11eb-9c5b-4fc9b6d03a33.JPG)

### Introduction to RISC V
   
   RISCV is the istruction set architecture or architecture of computer or language of the computer. Any application in our system is first compile by the OS into its respspective istruction set RISCV CPU core (picorv32a) in our case, which is basically the assembly language in hexadecimal. This hexadecimal instructions set is then converted into machine language of 1's and 0's which is then fed into the layout to obtain the desired output. The RTL serves as an interface between the assmebly laguage and layout. RTL converts the input assembly lanhuage into a syntesized netlist of logic gates and there after the physical design implentation or the standard RTL to GDSII flow occurs. 

![12-11](https://user-images.githubusercontent.com/63381455/106311580-62a6b200-628b-11eb-8120-bf4e77a6edca.JPG)

### OpenLANE ASIC design flow

For designing of an open source ASIC design flow, three components are required the RTL IP's EDA tools and PDK. Until the recent past the RTL Ip's and EDA tools are readily available however open source pdk was out of question. PDK or process design kit are cluster of data files and ducuments which serves as an interface between the designers and foundry. The PDK includes technology infomation, device models, design rules, IO libraries, digital standard libraries etc required for fariicating a process for the EDA tools used in designing an IC. PDK are traditionally close-source with lots of sensitive information and its comes with non-disclosure agreement and  thus a major limitation for open source digital ASIC design. This limitation brought Google and skywater together and they came up with the first ever open source PDK skywater 130nm on June 30, 2020. OpenLANE is built around Skywater 130nm process and is capable of performing full RTL2GDS flow as shown in figure below. 


![Openlane_flow](https://user-images.githubusercontent.com/63381455/106347844-8c8dc200-62e7-11eb-9f81-1baad994bcd8.JPG)

As shown in the figure above, a number of stages and sub stages are carried out in openLANE. By default all the stages of the flow run interatively in a sequence and number of open source EDA tools are used for performing various fuctionalities. OpenLANE integrate the various open source tools in a single PnR flow. The various tools required for interative flow of openLANE along with their functionalities are mentioned below.

- Synthesis 
    - Yosys - generates gate level netlist
    - abc - performs technology mapping
    - OpenSTA - performs pre layout static timing analysis to generate timing reports
- Floorplan
    - init_fp - Defining the core area for the macro as well as the cell sites and the tracks 
    - ioplacer - Places the macro input and output ports
    - pdn - Generates the power distribution network
- Placement    
    - RePLace - Performs global placement
    - OpenDP - Perfroms detailed placement to legalize the globally placed components
- Clock tree synthesis
    - Triton CTS -Synthesizes the the clock tree network 
-  Routing
    - FastRoute - Performing global routing to generate a guide file for the detailed router 
    - TritonRoute - Performing detailed routing 
-  GDSII Generation
   -  Magic - Streaming out the final GDSII layout file from the routed def 
   
### Part 2 - Det familiar with EDA tools 
Inorder to access the labs, labs instances is present in VSD_IAT flow which redirects to a vnc network. The vnc network allows us to remotely access the ubuntu terminal for performing the lab instances. The file required for carrying out the workshop are present in the path /work/tools/openlane_working_dir/. For running the openLANE flow, the treminal should be in the openlane path, /work/dir/openlane_working_dir/openLANE_flow. All the steps are to carried out along this path.

### Invoking openLANE

The openLANE can be invoked in a non-interactive/autonomous flow or interactive flow. For invoking openLANE in interactive mode we need to type the commannd: 

```bash
./flow.tcl -interactive  
```
### Import package
OpenLANE tools requires a number of software dependencies and thus we need to import the package via the command:

```bash
package require openLANE 0.9
```

 ### Prepare design
 
 The prepare design stage is use to prepare a folder for our design.
 
 ```bash
 prep -design picorvv32a
 ```
  The above will create a folder runs in the ./design/picorv32a/runs for maintaining our design which by default will create a folder with date folder name after each run. The folder is iteratively created after each run. Howeevr if we want a single folder be created and each run thet particulare folder is updated we need to use the -tag switch in the prep command which is:  
  
  
  ```bash
  prep -design picorvv32a -tag Geeti
  ```
  ![3](https://user-images.githubusercontent.com/63381455/106351788-6296c880-6304-11eb-995c-cf4593dbaad7.JPG)
  
  The figure showing the above three steps is as shown in figure above.
 Also for overwriting the runs for new run we need to use the siwtch -overwrite as follows. The above will erase all the changes in the runs folder and needs to be re done.
 
 ```bash
 prep design picorv32a -tag First -overwrite
 ```
 # Day 2
 ## FloorPlan
 During floor planning a number of parameters are to de set. A well define floor plan leads to an ASIC design with high performance and optimunm area. The parameters set during floor plan are:
 - Core and die area
 - Utilization factor
 - Aspect ratio
 - Placment of macros
 - Power planning (done later in openLANE
 - Placement of input and output pins
  
 ### Core area and die area :
 
 While defining the core and die area the dimensions of the standard cells are to be considered inordet to arrive to an optimum size. After finding the dimmensions of the standard cells all of them are clubbed to together the get the overall dimension. In the core area the fundamental logic is placed wheras 'die', which consists of the core is a small semiconductor specimen on which the fundamental circuit is fabricated. 
 
  ![1-1](https://user-images.githubusercontent.com/63381455/106354196-e9a06c80-6315-11eb-9b10-d7a2f387026e.JPG)
  
###  Aspect ratio and utilization factor :

The two are important description of floorplan. Aspect ratio is the ratio of the height of the core by the width of the core. It bascically defines the shape of a chip, aspect ratio = 1 means the chip is square in shape and any other value indicates that the chip is rectangle. Utilization factor defines the area covered by the netlist in the core. A utilization factor of 1 indicates that the entire core is occupied by the core which is not at all feasible. A utilization factor of 0.5 - 0.7 is considerd as optimal. 

![5](https://user-images.githubusercontent.com/63381455/106354493-338a5200-6318-11eb-93a0-80552f1135c8.JPG)

### Concept of pre placed cell : 

Pre placed cells allows the granulizing of a larger design for usage whenever required in the design. Pre placed cells or Macros and IP's have user defined loactions and hence are placed on the chip before automated placement and routing. The pre placed are implemented once and can be instantiated mulitiple times in the netlist.

### Decoupling capcitors : 

Decoupling capacitors are placed locally around the pre-placed cells. The decoupling capacitor is a large capacitor completely full of charge whose voltage is equivalent to the power supply. During switching activity the decoupling capacitor decouples the circuit from the main supply and provides the necessary voltage required by the pre placed cells. The importance od decoupling capacitor is due the fact tha voltage drop across the wire interconnects between the main supply and the circuit may result in unwanted noise margin which may cause the preplaced cell ti be in the undeined region and behave abnormally.

![7-8](https://user-images.githubusercontent.com/63381455/106355373-6daa2280-631d-11eb-95b8-d802c10bcef1.JPG)

### Power planning : 

As all coupling capacitors present in the circuit demands the power suppy simultaneosly a single power supply cannot adhere to the demands which reult in noise in the circuit cause due to voltage groop or ground bounce. Hence, power planning is very important part of floor planning. During this stage mulltiple power supply are placed in a chip for proper fuctioning. One structure for the power planning is the mseh structure where multiple power and groud lines are arrange in horizontal abd vertical manner as shown in figure below.

![11](https://user-images.githubusercontent.com/63381455/106355851-b0b9c500-6320-11eb-935a-9cf3d24f13b1.JPG)

### Pin placement : 

For proper pin placement the connectivity information coded using verilog/vhdl language called netlist is considered. The location of input and output pin depensd upon the conncetivity requirement and the designer. However, the basic trend is to select a location which results in reduce connectivity length. Optimal pin placement is done taking care about the less buffering and less amount of power consumption. The size of the clock pins are wider as compared to the other data pins.  The locgiical cell blockage is done to make sure that the automated PnR does not used the are for placement of cells.

![12-13](https://user-images.githubusercontent.com/63381455/106356122-232ba480-6323-11eb-909e-3f8f3c279698.JPG)

### Floorplan in openLANE
  To run floorplan we simply need to run the command.
  
  ```bash
  run_floorplan
  ```
  Prior to running floorplan we can check the different switches available for floor planning at the location ./openlane/configuration. This folder also contains the different tcl files for each step where the default values are taken. The switches required as per design can be set at ./openlane/design/picorv32a/comfig.tcl file. Some configurations are also mentioned in sky130A_sky130_fd_sc_hd_config.tcl file. The precedence of tcl file is as follows 
 
 1. sky130A_sky130_fd_sc_hd_config.tcl file (./openlane/design/picorv32a/)
 2. config.tcl (./openlane/design/picorv32a/)
 3. floorplan.tcl (./openlane/configuration)
 
 The output of floorplan is a def file find in ./designs/picorv32a/runs/Geeti/results/floorplan/picorv32.floorplan.def. Inorder to view the output of floorplan in magic, we need to provide 3 files
 1. sky130.tech file (./pdks/sky130A/libs.tech/magic/)
 2. merged.lef file (./designs/picorv32a/runs/Geeti/tmp)
 3. picorv32a.floorplan.def (./designs/picorv32a/runs/Geeti/results/floorplan)
 
 To invoke magic we need to use the command:
 ```bash
 magic -T <tech read path> <lef read path> <def read>
 ```
 
 
 





