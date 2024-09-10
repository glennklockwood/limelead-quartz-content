---
aliases:
  - Cray EX4000
  - Cray EX2500
---
Cray EX4000 is one of the high-density, liquid-cooled cabinets created by Cray for its Shasta program.

Each EX cabinet has:[^1]

- 8 compute chassis
- 64 compute blade slots total (8 per chassis)
- 4 power shelves with 1 PDU each
- 4 power input whips
- 64 switch slots

Compute blades slot in the front vertically, and switch blades slot in the rear horizontally:

- eight blades can connect to up to eight switches within a single chassis
- hot and cold water connections are in the front
- all [[Slingshot]] networking is in the rear

This is the front of an EX4000 cabinet I took at SC19:

![[Cray EX4000 front.jpg|360]]

You can see the hot and cold water lines (red and blue hoses) and the eight chassis: four stacked vertically on the left and right halves of the cabinet. Each chassis has vertical compute blades into which the cooling hoses are connected.

Here is the back of that same cabinet:

![[Cray EX4000 rear.jpg|360]]

You can see extensive [[cables and connectors|DAC]] copper cabling that carries the 200G [[Slingshot]] between switches within the rack. The teal optical leave the cabinet and connect to other [[Slingshot]] groups.

For a list of all the blades compatible with Cray EX, see [HPE Cray Supercomputing EX](https://www.hpe.com/psnow/doc/a00094635enw.html).

[^1]: [HPE Cray EX Supercomputer](https://support.hpe.com/hpesc/public/docDisplay?docId=a00109703en_us)