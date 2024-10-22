---
title: "Azure Accelerated Networking: SmartNICs in the Public Cloud"
tags:
  - paper/microsoft
---
Source: [Azure Accelerated Networking: SmartNICs in the Public Cloud - Microsoft Research](https://www.microsoft.com/en-us/research/publication/azure-accelerated-networking-smartnics-public-cloud/). It is a paper published by [[Microsoft]] about its [[Azure SmartNIC]] technologies.

> GFT [[flow|flows]] are defined based on the VFP unified [[flow|flows]] (UF) definition, matching a unique source and destination L2/L3/L4 tuple, potentially across multiple layers of encapsulation, along with a header transposition (HT) action specifying how header fields are to be added/removed/changed


> a physical core (2 hyperthreads) sells for \$0.10-0.11/hr1 , or a maximum potential revenue of around \$900/yr, and \$4500 over the lifetime of a server


> all policies can be enforced in VFP software on the first packet of a new TCP/UDP [[flow]], after which the actions for that [[flow]] can be cached as an exact-match lookup


> stateless offloads for NVGRE and VxLAN encapsulation for virtual networking scenarios for Azure in the 2010s


> SRIOV is an example of an all-or-nothing offload


> stateful [[flow|flows]] tend to be mapped to only one core/thread to prevent state sharing and out-of-order processing within a single [[flow]]


> scalability for 100GbE, 200GbE, and 400GbE looks bleak


> modern NIC, the packet processing logic is not generally the largest part. Instead, size is usually dominated by SRAM memory (e.g. to hold [[flow]] contexts and packet buffers), transceivers to support I/O (40GbE, 50GbE, PCIe Gen3), and logic to drive these interfaces (MAC+PCS for Ethernet, PCIe controllers, DRAM controllers), all of which can be hard logic on an FPGA as well


> Another option was to build a full NIC, including SRIOV, inside the FPGA — but this would have been a significant undertaking (including getting drivers into all our VM SKUs), and would require us to implement unrelated functionality that our currently deployed NICs handle, such as RDMA


> cable connects the NIC to the FPGA and another cable connects the FPGA to the TOR


> first generation [[Azure SmartNIC]], deployed in all Azure compute servers beginning in 2015


> make the SmartNIC appear as a single NIC with both full SR-IOV and GFT support


> When the FPGA receives an exception packet, it overloads the 802.1Q VLAN ID tag in the packet to specify that it is an exception path packet, and forwards the packet to the hypervisor vPort. VFP monitors this port and performs the necessary [[flow]] creation tasks after determining the appropriate policy for the packet’s [[flow]]


> The Parser stage parses the aggregated header information from each packet to determine its encapsulation type.


> When the FPGA detects termination packets such as TCP packets with SYN, RST or FIN flag set, it duplicates the packet — sending the original packet to its specified destination, and an identical copy of the packet to the dedicated hypervisor vPort. VFP uses this packet to track TCP state and delete rules from the [[flow]] table


> Action stage, which takes the parameters looked up from the [[flow]] table, and then performs the specified transformations on the packet header. The Action block uses microcode to specify the exact behaviors of actions, enabling easy updates to actions without recompilation of the FPGA image


> software-programmable Quality-of-Service guarantees can be implemented as optional components of the processing pipeline


> [[flow|Flows]] are then updated lazily by marking the first packet of each [[flow]] as an exception packet


> accomplished online serviceability by turning off hardware acceleration and switching back to synthetic vNICs to maintain connectivity when we want to service the SmartNICs


> when the VF comes up, our synthetic NIC driver, the Hyper-V Network Virtual Service Consumer (NetVSC), marks the VF as its slave, either by using the slave mode present in the kernel for Linux VMs, or by binding to the NetVSC’s upper NDIS edge in Windows VMs. We call this transparent bonding — the TCP/IP stack is bound only to the synthetic NIC


> all our common RDMA applications are designed to gracefully fall back to TCP anyways


> app-level transparency for RDMA serviceability remains an open question for the future if apps ever take a hard dependency on RDMA QPs staying alive


> P4 could potentially serve as a way to specify some of the GFT processing [[flow]]
