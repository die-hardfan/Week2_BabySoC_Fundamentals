# System on Chip (SoC) Fundamentals

An overview of SoC design flow was seen in Week0. This will explore the theoretical concepts behind SoC, its components, and its pros and cons.

<details>
  <summary> What is a System-on-Chip (SoC)? </summary>
//add an image here
**A system-on-chip is an integrated circuit that integrates all of a system’s required components onto a single piece of silicon.**

## Evolution of ICs into SoC

A system is a meaningful interconnection of various devices. A relevant context here would be a _computing_ system, which is a system capable of processing, storing, and communicating information (or data). 
(Henceforth, when _system_ is mentioned, it refers to _computing_ system)
Initially, each component in such a system had its own IC or chip. These components would later be interconnected on a PCB to form a system. According to Moore's law, as the transistor sizes shrank, it was possible to make the chips smaller with added functionality. 
To decrease the size of the system as a whole, the components were instead interconnected and fabricated into a single piece of silicon wafer. This came to be known as System on Chip, since we now had an entire _system_ fabricated on a _chip_ (or IC). 
On an SoC, we usually refer to “components” when discussing the system architecture (CPU, memory, interconnect, peripherals, other IP blocks). But when writing RTL or synthesizing the design, we deal with “modules” (the code-level representation of those components).
An IC does a specific low-level task (e.g., an AND gate IC, a Power management IC, an Op-Amp IC), but an SoC can do a high-level task (e.g., SoCs in Wearable devices, Automotives) or be general purpose (e.g., SoCs in smartphones, tablets). 

## Why SoC?

Typically, we want: "more performance, less power, less area", and SoCs manage to achieve some improvement in all three aspects relative to a traditional system. 

### 1. Area 
   - Since components are now fabricated on the same wafer, the "real-estate" consumed by packaging, board-level interconnects, etc., is saved. In this space, additional functional components can be added.
   - Increases functionality, portability
### 2. Performance
   - Components are now closer to each other (cuz no packages), so the interconnect delay is decreased (physically)
   - This can decrease the total delay, increase slack, or increase the operating frequency (if slack remains the same as before SoC integration)
   - Frequency of operation is the most direct measure of performance of any system, hence increased frequency increases performance
### 3. Power
   - One source of power dissipation is the I<sup>2</sup>R loss in the interconnects. We know, R ∝ L/A (L: length of interconnect, A: cross-section area of interconnect), so longer interconnects offer more resistance. 
   - Interconnects also offer capacitance. And for parallel plate capacitors, C ∝ L*W (L: length of interconnect, W: width of interconnect). Thus, longer interconnects offer more capacitance.
   - More capacitance requires more current to be charged faster (else delay increases with L). We know that P = VI, hence more current leads to increased power consumption.
   - On-chip interconnects are much smaller than off-chip interconnects. Having SoCs reduces the off-chip interconnections required, thus reducing I<sup>2</sup>R losses and power consumed.
### 4. Reliability
   - Off-chip interconnections are more vulnerable to signal integrity issues. On-chip interconnections are part of a chip, thus enclosed within robust packaging that provides necessary protection.
   - This makes SoCs more reliable, a quality that is mandatory in critical applications like automotives, healthcare, etc.
### 5. Customization
   - With hard and soft IP blocks available, SoCs can be highly customized to suit the application. An SoC (customized for an application) becomes highly application-specific (in a sense) and hence less likely to be used for different applications.
   - But, there are general-purpose SoCs available. 
     
##  Why not SoC?

1. Single point of failure: With all components in a single chip, a failure in one component affects the entire system.
2. Time to market: When compared to off-the-shelf components, designing custom SoCs requires more expertise and specialized tools with increased development time and costs. 
3. Mixed analog/digital: As all the components on an SoC are manufactured with a single process technology, there is no option to use optimal technology (often different from the one being used) for the analog sections. This leads to reduced analog performance and makes SoCs better suited for digital applications.
4. Flexibility: An SoC is ideally suited to its intended task but has limited scope to be applied to any other task.
  
</details>

<details>
  <summary> Components of a typical SoC </summary>

  //add an image here
  
A typical computing system has a programmable processor, on-chip memory, peripherals (for interfacing with the outside world), and necessary analog components (e.g., oscillators for clock generation). 
An SoC integrates all of them into a chip with additional functional units for accelerating specific tasks. 

### 1. Processor (CPU)
  - Responsible for data processing tasks from basic arithmetic calculation (simple) to running applications (complex) 
  - Multiple cores may be present in high-end systems
### 2. Memory
  - In simple systems, RAM and ROM are used for storing intermediate results or system software (firmware) necessary for the smooth functioning of the system
  - In complex systems, levels of cache memory hierarchy can be included on chip
### 3. Peripherals
  - Any component used to extend the functionality of the processor is a peripheral
  -  Controllers and software required for interfacing with components off-chip are necessary
  - Examples include ADC/DAC, GPIO, etc., in simple SoCs, to USB controllers, memory controllers, etc., in complex SoCs
  - **Additional Functional Units** like DSPs, GPUs, Crypto engines, etc., can be used to perform/accelerate specific tasks
### 4. Analog components
  - Oscillators are necessary for clock generation (for any sequential digital module to operate, eg, a processor)
  - Filters, power management circuits, transceivers, etc., can be included as well.
### 5. Application-specific features
  - Wi-fi, Bluetooth modules, opto/micro electro-mechanical systems (O/MEMS), or any IP blocks can be included based on the target application, if necessary.
  - Including reconfigurable logic (FPGA) is a possibility as well.

</details>

---

# BabySoC Fundamentals

## What is VSDBabySoC? 
VSDBabySoC is an open-source SoC that integrates 3 components: a 5-stage pipelined RISC-V processor, a Phase Locked Loop (PLL), and a Digital to Analog Converter (DAC). It was delivered by a VSD-HDP intern and is completely open-source. The goal is to test 3 open-source IPs and calibrate the analog components. 

<details>
  <summary> Components of VSD BabySoC </summary>
</details>

<details>
  <summary> Why BabySoC is a simplified model for learning SoC concepts </summary>
</details>

<details>
  <summary> The role of functional modelling before RTL and physical design stages </summary>
</details>

---

<details>
  <summary> References </summary>

Reference for SoC fundamentals:
- [Github repo](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)
- [Website](https://www.ansys.com/blog/what-is-system-on-a-chip)
- [IEEE Tutorial Paper](https://www.scribd.com/document/725357175/systemonchip-design)
  
Reference for VSDBabySoC:
- [Github repo](https://github.com/manili/VSDBabySoC/tree/main)
</details>

