---
aliases: []
---
PCIe 6.0 supports 64 GT/s[^pci-sig] or 128 GB/s per direction on an x16 connection.[^rambus] A lot of PCI-SIG collateral claims 64 GT/s is 256 GB/s, but they include 128 GB/s in both directions when they make this statement since PCIe has always been full duplex.

The above implies that 1 GT/s = 1 Gbit/s, or each transfer is 1 bit. I don't really understand how this works.

It is the first PCIe generation to use [[PAM4 and NRZ#PAM4|PAM4]] signaling, forward error correction, and flit-based encoding.

[^pci-sig]: [PCI Express 6.0 Specification | PCI-SIG (pcisig.com)](https://pcisig.com/pci-express-6.0-specification)
[^rambus]: [PCIe 6.1 - All you need to know about PCI Express Gen6 - Rambus](https://www.rambus.com/blogs/pcie-6/)