# VSD-IAT openLANE workshop

![1](https://user-images.githubusercontent.com/63381455/106157595-e2a81b80-61a8-11eb-8880-0fdebcb7cb2c.JPG)
 
# Table of contents

- [Overview](#overview)
- [Pre requisite](#Prerequisite)
- [Introduction](#Introduction)
- [Day 1 Inception and introduction of openlane tool openLANE and skywater 130 nm](#day)
  - [Introduction to QFN -48 package, chip, pads, core, die, IPs](#Imtro)
  - [Introduction to RISCV](#RiscV)
  - [OpenLANE design ASIC flow](#soc)
  - [Part -2 Get familiar with EDA tools](#familiar)   
  - [Invoking openLANE](#invoking)
  - [Import package](#import)
  -[Prepare design](#prepare)
- [Day 2 Floorplan and placement](#Day2)
  - [Floorplan](#Floorplan)
  - [Cell core and die area](#Cell)
  - [Aspect ratio and utilization factor](#Aspect)
  - [Concept of pre placed cells](#Concept)
  - [Decoupling capacitors](#Decoupling)
  - [Power planning](#Power)
  - [Pin placement](#Pin)
  - [Cell design flow and characterization](#designn)
- [Day 3 Design library cells and ngspice characterization](#Day3)
  - [Design library cells](#Designing)
  - [Spice deck extraction from magic](#Spice)
  - [DRC check](#DRC)
- [Day 4 Pre layout timing analysis and clock tree synthesis](#Pre)
  - [LEF file](#lef)
  - [Fixing slack violations](#slack)
  
  



# Overview

This repository is about the workshop conducted by VSD-IAT on advance physical design using the emerging trend of open source EDA tool openLANE and open source pdk sky130. In this workshop VSD provides overall training through interactive tutorials on the fundamentals of physical design as well as in hand experience on the use of the openLANE/sky130 flow . OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII .https://github.com/efabless/openlane.

# Pre requisite
 As for as the workshop is concerned the only requirement is the registered email id. VSD-IAT is an expertly build cloud training platform with zero restriction on using the platform across all time zones. The workshop design method provides freedom for use of the open source tools on new designs after the workshop https://www.vlsisystemdesign.com/vsd-iat/. 
 
However, for local use of the openLANE tool a set of instructions are to be followed. The initial requirement is an Ubuntu based system with at least 25GB+disk space. This repository https://github.com/nickson-jose/openlane_build_script gives the detailed instruction for building openLANE into the local system.
 
 # Introduction
 
Verilog RTL is basically a design written in Verilog Language which is a high level language. Either written in behavioral or structural form famously.
GDSII is the database file format which industry follows as a standard format for exchange of data related to IC layout. It is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form. The workshop starts with the self assessement process via some rapid fire question which assists in determining the understanding of the process follwed by the tutorials to learn the concepts. The main focus of this workshop is to use openLANE inconjuction with Google SKywater 130nm for full open source ASIC design.
 

# Day 1 Inception and introduction of openlane tool openLANE and skywater 130 nm

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
   
###  Get familiar with EDA tools 
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
 # Day 2 Floorplan and placement
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
 
 The output of floorplan is a def file find in ./designs/picorv32a/runs/Geeti/results/floorplan/picorv32.floorplan.def which defines the area of the chip. Inorder to view the output of floorplan in magic, we need to provide 3 files
 1. sky130.tech file (./pdks/sky130A/libs.tech/magic/)
 2. merged.lef file (./designs/picorv32a/runs/Geeti/tmp)
 3. picorv32a.floorplan.def (./designs/picorv32a/runs/Geeti/results/floorplan)
 
 To invoke magic we need to use the command:
 ```bash
 magic -T <tech read path> <lef read path> <def read floorplan> &
 ```
 ![39](https://user-images.githubusercontent.com/63381455/106362267-e8883300-6347-11eb-9c76-7bb748ebfa55.JPG)
 
 ### Placement :
 After the floorplan the next stage is the placement stage. In thoi stage the placement of standard cell is done . Placement in openLANE is a two step process global placement and detailed placement. Global placement is a an optimize one where the reduction in the wirelength by half power wavelength is done. There after global placement is done which is a legal placement adhereing to the global placement optimozation. The output of the placement stage is also adef file obtained at the placement folder inside the result folder. Again the output of the placement stage is viewed using the magic tool
 
 ```bash
 magic -T <tech read path> <lef read path> <def read placement> &
 ```
![global_placement](https://user-images.githubusercontent.com/63381455/106393618-28b9e500-641e-11eb-8ae5-289a28a754ad.JPG)

# Cell design flow aad characterization

Cell design flow consist of three parts input, design steps and output as shown in figure below.

![25](https://user-images.githubusercontent.com/63381455/106358822-8e31a700-6334-11eb-9dc8-4cde38b566fd.JPG)

## Characterizatiion
 Standard ccll library consists of cells with diffirent functionality, different threshold voltage, different drive strength and different sizes. Based on the design requirement we can use the cell. These cells are needs yo be characterized by the liberty files to be used by the synthesis tool for optimal circuit arrangement. The cell are characterize by application software GUNA. The characteization flow are as follows.
 
1.	Read the model file
2.	Read the extracted spice netlist
3.	Recognize the behavior of the buffer
4.	Read the sub circuit of buffer
5.	Read in the necessary power supply
6.	Apply the stimulus
7.	Provide the necessary output capacitances
8.	Provide the necessary simulation command
9. Feed in the characterisation file containing steps 1-8 into the GUNA software whose output is .LIB file containing the timing, noise and power characterization

# Day 3 Design library cells and ngspice characterization
 
### Designing library cells

1. Git clone of vsdstdcelldesign

For the prpose of the workshop already designed standalone standard cell is considered. For that vsdstdcelldesig is git clone form https://github.com/nickson-jose/vsdstdcelldesign. A folder is created inside the openLANE folder. It consists of a layout file sky130_inv.mag which can be viewd in magic layout window as 
`magic -T sky130A.tech sky130_inv.mag &`. The color pallete on the right side of the magic layout window describes the different layers and and components used in the layout window. 

![2](https://user-images.githubusercontent.com/63381455/106362371-6f3d1000-6348-11eb-8465-385ca864306f.JPG)

### DRC Check

Magic layout allows to view DRC check if any. The DRC error are defined by numerical value. A DRC value = 0 indicates no error and any other value defines error. The DRC error can be removed by using the required color palette.

![3](https://user-images.githubusercontent.com/63381455/106362501-35b8d480-6349-11eb-869b-cf71dd1eca76.JPG)


 ### Spice dexk extraction from magic
  
To extract the standard cell we go to the tckon window and type the following set of commands

- extract all 
- ext2spice cthreh 0 rthresh 0 (create an sky130_inv.ext file)
- ext2spice sky130_inv.spice

The spice is open and the necessary changes are done and there after ngspice is used to view the plots. The output plot is then plotted by executing  the command plot as
`plot <out> vs time <a>`. From the plot we can find out three important parameters of the plot rise time, fall time and propagation delay. Ris time is the time taken by the signal to move from 20% to 80% of its maximum value. The difference of the two the rise transition time and vice versa for fall transition time. Propagation delay is differnce between of 50% of signal output to 50% of the signal input.

```bash
Transition time = time(slew_high_rise_thr) - time(slew_low_rise_thr)
Propagation delay = time(out_*_thr) - time(in_*_thr)
```
![1](https://user-images.githubusercontent.com/63381455/106362343-3ef57180-6348-11eb-8b74-1b9d63939988.JPG)

The desired values can be retrive from the graph by right clicking on the part of the graph and placing the cursor on the point. The x axis and y axis value are diplayed in the terminal inside ngspice.

# Day 4 Pre layout timing analysis and clock tree synthesis

### LEF file

We need to extract the lef file from the .mag file. The lef file is needed to plug in the custom built inverter into the picorv32a core. The LEF file contains information regarding the IO ports, the ground ports and power ports. It does not contain the logical interconnection information. For the PnR flow of the standard cells certain guidelines are to be followed. 

 a.	The input and output port must lie in the intersection of horizontal and vertical track – li1 metal layer.
 b.	The width of the standard cell must be odd multiple of track pitch.
 c. c.	The height of the standard cell must be odd multiple of vertical track pitch.
 
 The path where the tracks info are located is -  cd pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd – vimtracks.info. The track info file are obtained by using the grid info command on the tckon window  `Grid 0.46um 0.34um 0.23um 0.17um`.
 
 ### Steps to covert magic cell layout to standard LEF format
 1.	Port definition are required to extract the lef file only they have no work in magic. Now save theSave sky130_vsdinv.mag
```bash
Magic –T sky130A.tch sky130_vsdinv.mag
```
2.	In tckon window type – lef write (lef file will be created with the same name as the mag file `sky130_vsdinv.lef'

Prior to plugging the custom made `sky130_vsdinv.lef` into picorv32 core, all the design files are to copied to ./design/picorv32a/src/ folder. Also the timing lib viz sky130_fd_sc_hd_typicsl.lib, sky130_fd_sc_hd_fast.lib sky130_fd_sc_hd_slow.lib and tyoical,lib file in ./opemLANE_flow/vsdsedcelldesign/lib/ folder is to be copied to ./design/picorv32a/src/ folder. We use the cp (copy) command to do so
```bash
cp <source path> <destination path>
cp ./opemLANE_flow/vsdsedcelldesign/lib/sky130_fd_sc_typicsl.lib ./design/picorv32a/src/
```
OpenLANE has the ability of making changes in the fly and thus we can the custom design standard cell into the cpu core and then rerun the floorplanning and placement stage without invoking the tool. Inorder to include the design into the picorv32a core we used the following commmands to obtain the sky130_vsdinv.lef file after invoking the openLANE_flow. We need to set the necessary liberties in the config.tcl file in picorv32a folder.

```bash
./flow.tcl -interactive
```
```bash
package require openlane 0.8
```
```bash
prep -design picorv32a -tag Geeti -overwrite
```
The `overwrite` switch shall delete whatever contents are available in the Geeti folder the floorplan, placement etc is to be executed over again. We add the follwing commands to make the neccessary change on the ./Geeti/tmp/merged.lef file. The commands are
```bash
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
```
```bash
add_lefs -src $lef
```
Next the synthesis is again executed and it comes to notice that the slack is negative which is not acceptable.
```bash
run_synthesis
```
![3](https://user-images.githubusercontent.com/63381455/106393301-556cfd00-641c-11eb-9507-150e86e27a5f.JPG)

Here we invoke the magic tool to check whether our custom made buffer is added into the design via the command `magic -T <tech read> <lef read> <def read>`. The magic window is open and we can scroll into the window to check.

![4](https://user-images.githubusercontent.com/63381455/106433470-462a9580-6496-11eb-966f-a1f5d19b9a94.JPG)


### Fixing slack violations

Slack violations are important issues and has to be rectified for optimum performance of the design. For this the static timing analysis is to be done to ensure that the the set up and hold slack are maintained within the prescribed limits viz 0 or above. The timing performance of the design is analyse using the openSTA tool. Prior to performing the cts the timing performance of the design is to be checked usign sta tool. For this the configuration file `pre_sta.conf` is invoked by the sta tool . THe configuration file along with the design sdc file is to copied into the `./picorv32a/src/` folder. After invoking the pre_sta.conf file in sta tool`sta pre_sta.conf`, the timing information of the design which includes the skew, delays, data arrival time, data required time,fao etc is defined which is the one obtained during the synthesis stage.

Now as the design requirement viz set up slack and the hold slack should be 0 or positive, certain design correction steps are to be done which includes.

1. Setting the `SYNTH_STRATEGY` switch
2. Setting the `SYNTH_BUFFERING` switch
3. Setting the `SYNTH_SIZING` switch
4. Optimizing the maximum out fanout `SYNTH_MAX_FANOUT`

All the above are changed using the openLANE tool by using `set ::env(switch) <required parameter>`. Now we again run the synthesis in the openSTA enviroment and observe the set up and hold timing slack and there is good improvement in the values though not the required value.

![7](https://user-images.githubusercontent.com/63381455/106428291-bb926800-648e-11eb-8801-a5684044ce6b.JPG)

Next further improvement is to be done. For that the table is observed where certain buffer nets whose capacitance as well as the delay is also high resulting in subsequent increase in the delay values is considered. The number of nets driven by the particular buffer is viwed inside the openSTA using `Report_net –connections <net>`. As the buffer used sky130_fd_sc_hd_buf_1 is of low drive strength it is replaced by higher buffer drive strength sky130_fd_sc_hd_buf_4, sky130_fd_sc_hd_buf_8 or sky130_fd_sc_hd_buf_16. The replacement is done using sta command replace as `replace <cell instance> <lib cell>`. For any information regarding a particular command, `help <command name>` can be used. Thereafter we can check whether the changes has resulted in improvement or not using `Report_checks –fields {net cap slew input pins}  -digits 4`, where the digit attribute is optional, it simply shows the values upto 4 decimal places. The report check shows improvement in the slack timing but still not proper and we can try to further improve the timings.

![2](https://user-images.githubusercontent.com/63381455/106431369-29d92980-6493-11eb-9636-f9384aaafadf.JPG)

After improving the slack to as minimum value as possible the obained netliss is to be changed in the verilog file for further use by the openLANE flow. The verilog file in the synthesis path is to be updated using `write_verilog <path>`

Finally the floorplan and placement is again executed in the openLANE flow. The next stage is clock tree synthesis. During this stage the cts will add the clock buffers and the netlist will be modified. 
