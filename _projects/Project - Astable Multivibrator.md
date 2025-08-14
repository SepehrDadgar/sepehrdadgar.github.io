
the first and easiest project for most people when they want to get started with electronics is a astable multivibrator

infact the first ever electronics kit i made when i was 12 was this exact circuit but not many actually care to understand how this circuit actually functions

insert picture of the circuit from school


there are two ways to make this circuit use a 555 timer or use transistors for an analog solution

## 555 Method
### Pin Connections
![](555%20pin%20connection.png)
Connect $R_1$ between VCC and DIS; $R_2$ between DIS and THR/TRIG node; $C_1$ from THR/TRIG to GND. OUT is your square wave.
### Operation
the capacitor charges through $R_1+R_2$ until $\frac{2}{3}$·
VCC (flip output), then discharges through $R_2$ via the discharge pin until $\frac{1}{3}$·
VCC, and repeats.
![|202x297](Project%20-%20Astable%20Multivibrator-1754887626077.png)
### Formulas
