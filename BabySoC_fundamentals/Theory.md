# System on Chip (SoC) Fundamentals

An overview of SoC design flow was seen in Week0. This will explore the theoretical concepts behind SoC, its components, and its pros and cons.

<details>
  <summary> What is a System-on-Chip (SoC)? </summary>
  
![IC vs SoC](/images/icvssoc.png)
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

![SoC components](/images/soc_components.png)
  
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
VSDBabySoC is an open-source SoC that integrates 3 components: a 5-stage pipelined RISC-V processor, a Phase Locked Loop (PLL), and a Digital to Analog Converter (DAC). A VSD-HDP intern delivered it and is completely open-source. The goal is to test 3 open-source IPs and calibrate the analog components. 

![VSDBabySoC](/images/babysoc.png)

<details>
  <summary> Components of VSD BabySoC </summary>
It contains 3 major components, the RISC-V processor, the PLL and the 10 bit DAC. 

### 1. RISC-V PROCESSOR
This is made at the RISC-V workshop RVMYTH, which uses TL-Verilog to code a 5 stage pipelined RISC-V processor. This is like the data-processing unit (with memory) of the SoC.

**WORKING**

### 2. PLL (Phase Locked Loop)
This is responsible for generating the stable clock signals required for the functioning of the processor and the DAC. This is like the necessary analog component within a SoC. 

**WORKING**

A **PLL** is a circuit that generates a stable output signal whose **frequency and phase are locked** to a reference signal.

![PLL](/images/pll.png)

1. **Reference signal:** The PLL compares its output to a reference clock.  
2. **Phase detector:** Measures the difference in phase between the output and reference.  
3. **Loop filter:** Smooths the phase difference into a control voltage.  
4. **Voltage-Controlled Oscillator (VCO):** Adjusts its frequency based on the control voltage.  

> The loop continually adjusts the VCO so that its output matches the reference signal in frequency and phase.
> Used to **generate clocks**, **multiply/divide frequencies**, and **synchronize signals**.  
> Works like a **self-correcting oscillator** that “locks on” to a reference.

```
module avsdpll (
   output reg  CLK,        // Output clock of the PLL
   input  wire VCO_IN,     // Input from VCO (not really used in this sim)
   input  wire ENb_CP,     // Charge pump enable (not used here)
   input  wire ENb_VCO,    // VCO enable (active-low)
   input  wire REF         // Reference given as real value
);
```


### 3. DAC (Digital to Analog Converter)
This is responsible for interfacing the BabySoC with other analog components if necessary. It takes the data stored in register `r17` (within the processor) and converts it into its analog value. This is like the peripheral for the SoC.

**WORKING**

The purpose of DAC is to convert digital data into desired analog form (voltage or current). This is done in various ways as given below. And the verilog models a 10-bit Voltage-Output DAC. 
```
   output      OUT;
   input [9:0] D;
   input       VREFH;
   input       VREFL;
```
The module converts the 10 bit `D` to an appropriate voltage value between `VREFH` and `VREFL`. 

![dac](/images/dac.png)

  <details>
    <summary> Types of DACs</summary>
    
  ### 1. Voltage-Output DAC (VOUT DAC)
  A voltage-output DAC produces an analog voltage proportional to the digital input code.
  The output voltage typically follows the relationship:
  V<sub>OUT</sub> = V<sub>REFL</sub> + (<sup>D</sup>&frasl;<sub>2<sup>N</sup> − 1</sub>) × (V<sub>REFH</sub> − V<sub>REFL</sub>)
  
  ### 2. Current-Output DAC (IOUT DAC)
  A current-output DAC generates an analog current rather than voltage:
   I<sub>OUT</sub> = (<sup>D</sup>&frasl;<sub>2<sup>N</sup> − 1</sub>) × I<sub>REF</sub>
  
  ### 3. Segmented DACs (Advanced Type)
  Split the input code into two segments:
    - MSB section: thermometer-coded (each unit source adds the same amount). Avoids large step errors; improves monotonicity and linearity.
    - LSB section: binary-weighted (smaller contribution). Reduces area and complexity.
  </details>
</details>

<details>
  <summary> Why BabySoC is a simplified model for learning SoC concepts </summary>
From the Components of SoC section, we know the main components of SoC are the processor, memory, peripherals, and necessary analog components. BabySoC has the spec that can meet the bare minimum qualifications of an SoC. It has a processor (very basic RISC-V core that follows RV32I), memory (data mem, instr mem, and reg file as part of the processor pipeline), a DAC as a peripheral, and a PLL as the necessary analog component. This is clearly as simple as an SoC can get, with all the essential components present. Thus, before moving on to actual SoC design, studying BabySoC thoroughly will build up on the most essential concepts required, like modelling analog components and their calibration etc.  
  
</details>

<details>
  <summary> The role of functional modelling before RTL and physical design stages </summary>

  <details>
    <summary> Detailed perspective</summary>
    
  ### what is functional modelling?
  An SoC is a mixed-signal system with digital and analog components. The development of digital and analog is fairly different. But when simulating SoC as a whole to check for functional correctness, analog (or other components like DAC, ADC) are modelled using Verilog. The purpose is to check the functional integrity of the system as a whole.
  Initially, even digital components are modelled using languages more abstract than Verilog, perhaps C/C++ or even TL-Verilog. 
  
  ### Role of functional modelling
    
  Most designs, digital or otherwise, require multiple iterations of testing and modifications. In describing the behaviour of the module (as opposed to its actual implementation in hardware), the time required to design, modify, and test (iteration time) reduces drastically. Once the most optimized form is ready at this level of abstraction, lower-level implementation starts, and any required optimizations (which are kept to a minimum because the iteration time increases as more details are included). 
  Behavioural code describes the function of the module on a simulation level.
  RTL code describes the implementation of the module on a circuit/hardware level (in the form of logic gates/standard cells).
  Layout (physical design) describes the module at the device level on the silicon itself (in the form of masks needed for fabrication).
  More details --> more design time (mainly cuz of the computation involved) --> increased time to market and decreased productivity. Functional modelling enables easier and faster design iterations. 
  
  >Functional modeling is a simulation-focused way of describing a circuit.
  >It describes what the circuit does, not how it is physically implemented in gates or transistors.
  >Focus is on behavior and correctness, not timing or synthesizability.
  >Often uses non-synthesizable constructs (like delays, real variables, system functions, etc)
  </details>

### In short
**Functional modeling** is a simulation-focused way of describing a circuit. It focuses on **what the circuit does**, not how it is physically implemented in gates or transistors. The emphasis is on **behavior and correctness**, not timing or synthesizability.

An SoC is a mixed-signal system with both digital and analog components. When simulating the whole SoC for functional correctness, analog or mixed-signal IPs (like DACs, ADCs, or PLLs) are often modeled in Verilog using behavioral constructs. Initially, even digital blocks may be described at higher levels of abstraction using languages such as C/C++ or TL-Verilog.

- **Faster iteration:** By describing only the behavior of modules, functional models allow designers to **test, modify, and verify system behavior quickly**, reducing design time and cost.  
- **Early verification:** Enables **system-level testing, software-hardware co-simulation, and integration of multiple IP blocks** before RTL is available.  
- **Support for mixed-signal components:** Analog and mixed-signal IPs can be simulated using `real` variables, delays, or system functions, which are generally **non-synthesizable**.  
- **Abstraction trade-off:** Higher abstraction in functional models → **faster simulation, easier debugging**, but less detail. RTL and physical design introduce more detail, increasing iteration time and computational requirements.  
- **Foundation for RTL and physical design:** Once functional behavior is validated, designers move to **RTL implementation** (synthesizable, gate-level) and **physical layout** (device-level, silicon masks), minimizing the risk of propagating errors downstream.

> Functional modeling reduces iteration time, enables early system-level verification, and provides a clear behavioral foundation before committing to RTL and physical design.

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

