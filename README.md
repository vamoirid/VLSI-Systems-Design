# VLSI-Systems-Design
 This repository contains all the assignments for the Academic Courses "VLSI-Systems-Design" taught in the Fall of 2019-2020 in Aristotle University of Thessaloniki - Electrical and Computer Engineering. 

The project was done with [Cadence Virtuoso](https://www.cadence.com/en_US/home/tools/custom-ic-analog-rf-design/layout-design/virtuoso-layout-suite.html) program with the student licenses that Cadence provides to our University and in collaboration with - [Stefanos Tsoukias](https://github.com/tsoukias)

### 1. Scematic Layout

---

The task consists of 10 transistors in total, 5 of which are p-MOS and the other 5 are n-MOS. The pins used in the circuit are:

* **Vdd**: is the circuit's power supply and is directly connected to the sources of 4 of the 5 p-MOSs, which are at the top of the schematic and then to the substrates of all p-MOSs in the circuit.
* **Vss**: is the ground of the circuit and is directly coupled to the source of 1 of the 5 n-MOSs, located at the bottom of the schematic and then to the substrates of all n-MOSs in the circuit.
* **Clk**: is the input pulse of the circuit. The circuit is designed to be level-triggered and not edge-triggered.
* **Vinp**: is the "positive" entry.
* **Vinn**: is the "negative" entry.
* **Outp**: is the "positive" output.
* **Outn**: is the "negative" output.

![schematic](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/CADs/Schematic.png)

### 2. Symbol

---

The symbol of this particular IC is quite similar to the symbol of a common Comparator or of an Operational Amplifier only modified to include additional pins in our circuit.

![schematic symbol](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/CADs/Symbol.png)

### 3. Working Principle

---

So in order to be able to control our circuit and select the appropriate transistor sizes, we built a testbench in which we changed parameters in order to find the circuit principle first and then how to optimize it. The components that make up the testbench are:

* **V0**: is the source that generates the circuit ground.
* **V1**: is the source that generates 1V of the circuit.
* **V2**: is the source that generates the circuit clock. It has a width of 1V, is a square pulse and has a frequency of 10MHz.
* **V3**: is the source that produces the "negative" circuit input. It is a sinusoidal signal with an initial phase of 0 degrees, a DC voltage of 0.5V and a width of 0.5V.
* **V4**: is the source that produces the "positive" circuit input. It is a sinusoidal signal with an initial phase of 180 degrees, a DC voltage of 0.5V and a width of 0.5V.
* **C0, C1**: capacitors that simulate the loads that the output gates have to drive.

So the whole circuit looks like this:

![testbench_schematic](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/CADs/Testbench.png)

The reason why these input sources were selected is because with this specific combination of parameters we obtain 2 sine waves between the values 0-1V which are always greater than one other than the crossing-point.

![plots_inputs](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/plots/Inputs_white.png)

Whereas the plotting of all the variables together gives us:

![plot_all](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/plots/All_white.png)

So having all the necessary graphics we can easily understand the operation of the circuit. When **Vinp > Vinn** is applied then **Outp = 1V** and **Outn =! CLK**, that is, the corresponding output of the larger input permanently has the value of 1V at its output while the other output gives the clock reverse pulse. So for **Vinn > Vinp** we have **Outn = 1V** and **Outp =! CLK**.

### 4. Testing and Final Sizes

---

The optimization of the circuit is mainly aimed at reducing the spikes that appear in the clockwise switches at the outputs. To do this, we changed the sizes of p-MOS and n-MOS with different values each time from 120 nm to 960 nm in p-MOS. However, the improvement in spike reduction was minimal to negative in some modifications, so we concluded that n-MOS would remain at 120nm and p-MOS at 240nm beyond 1 p-MOS which is not connected to power and it drives inputs from 2 n-MOSs and not some outputs which means it doesn't necessarily have to be the same size as the rest.

We also changed clock and input values from 10MHz and 1MHz to 100MHz and 10MHz respectively, up to 1GHz and 100MHz. What has been observed is that while the circuit works satisfactorily with a 10MHz clock when it is increased to 100MHz and 1GHz the circuit is now unable to follow these output changes.

### 5. Physical Layout

---

The physical design of the circuit was designed to initially look as schematic as possible in order to facilitate its design and subsequently in as few polycrystalline paths as possible to reduce line resistance. The final circuit is as follows:

![physical_final](https://github.com/vamoirid/VLSI-Systems-Design/blob/master/Graphs_Results/CADs/physical_final.png)

