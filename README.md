# VSD-IAT openLANE workshop

![1](https://user-images.githubusercontent.com/63381455/106157595-e2a81b80-61a8-11eb-8880-0fdebcb7cb2c.JPG)
 
# Table of contents

- [Overview](#overview)
- [Pre requisite](#Prerequisite)
- [Introduction](#Introduction)'
- [Day 1](#Day1)
- - [Introduction to QFN -48 package, chip, pads, core, die, IPs](#Imtro)
- - [Introduction to RISCV](#RiscV)
- - [SOC and openLANE](#soc)



# Overview

This repository is about the workshop conducted by VSD-IAT on advance physical design using the emerging trend of open source EDA tool openLANE and open source pdk sky130. In this workshop VSD provides overall training through interactive tutorials on the fundamentals of physical design as well as in hand experience on the use of the openLANE/sky130 flow . OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII .https://github.com/efabless/openlane.

# Pre requisite
 As for as the workshop is concerned the only requirement is the registered email id. VSD-IAT is an expertly build cloud training platform with zero restriction on using the platform across all time zones. The workshop design method provides freedom for use of the open source tools on new designs after the workshop https://www.vlsisystemdesign.com/vsd-iat/. 
 
However, for local use of the openLANE tool a set of instructions are to be followed. The initial requirement is an Ubuntu based system with at least 25GB+disk space. This repository https://github.com/nickson-jose/openlane_build_script gives the detailed instruction for building openLANE into the local system.
 
 # Introduction
 
Verilog RTL is basically a design written in Verilog Language which is a high level language. Either written in behavioral or structural form famously.
GDSII is the database file format which industry follows as a standard format for exchange of data related to IC layout. It is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form. The workshp starts with the self assessement process via some rapid fire question which assists in determining the understanding of yor poocess follwed by the tutorials to learn the concepts. The tutorial starts with the basic way of interacting with computers.
 

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

### SOC and openLANE



