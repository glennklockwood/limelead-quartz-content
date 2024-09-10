---
tags:
  - network
---
Minipack2 is a 4U modular switch developed by Meta and manufactured by Celestica.[^1]

![[Minipack2 front iso view.png|480]]

As shown in the image above[^2], it has eight Port Interface Modules (PIMs), and each PIM has:

- 512x 50G [[PAM4 and NRZ#PAM4|PAM4]] serdes lanes through a [[Tomahawk 4]] ASIC
- 16x [[cables and connectors|QSFP56]] 200G ports or 8x [[cables and connectors|QSFP-DD]] 400G ports

This provided a total of up to 128x 200G or 64x 400G ports.

Minipack2 was released as an OCP specification, and its configuration is detailed extensively in [Minipack2 Generic OCP spec][^2].

[^1]: [OCP: Meta updates Facebook's data center switches, moves to SAI open source API - DCD (datacenterdynamics.com)](https://www.datacenterdynamics.com/en/news/ocp-meta-updates-facebooks-data-center-switches-moves-to-sai-open-source-api/)
[^2]: [Minipack2_Generic_OCP_Specification_rev1.0 (opencompute.org)](https://www.opencompute.org/documents/minipack2-generic-ocp-specification-rev1-0-pdf)