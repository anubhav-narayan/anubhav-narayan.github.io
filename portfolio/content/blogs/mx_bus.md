---
title: "MX Bus Architecture"
date: 2024-09-21T11:30:34+05:30
draft: false
---
## Intro
To work with the new project machine extensible, I have designed a new bus to simplify and categorise the use of the signals between Avalon and AXI bus architectures, which I find to be "a bit too much" for my "simple machine".
Upon closer examination, I noticed that Avalon and AXI buses share several fundamental operations. By identifying these commonalities, I saw an opportunity to create a bus architecture with a smaller, more efficient footprint. This is where the MX Bus comes into play. The MX Bus is specifically designed to minimize the number of mandatory signals, simplifying the overall architecture. Additionally, it is flexible enough to be easily bridged to both the Avalon and AXI standards, making it an ideal solution for projects where simplicity is essential. It reduces the overall signal count of mandatory signals and can be easily bridged to both the standards.

## Signals and Roles
The bus architecture specifies a fixed clock and reset signal and a fixed transaction group or `txn` group. The channel has optional signals that can vary as per the design role of the bus.
### Clock and Reset

| **Signal** | **Direction** | **Size** | **Description** |
|:----------:|:-------------:|:--------:|:---------------:|
|`clk`       |In             |1         |The clock signal |
|`rst`       |In             |1         |The reset signal |

**Table 1. :** Clock and Reset

### Transaction `TXN` Group

| **Signal** | **Direction** | **Size** | **Description** |
|:----------:|:-------------:|:--------:|:---------------:|
|`txn_start` |Host -> Endpoint|1        |The signal when asserted by the host initiates a transaction|
|`txn_ack`   |Endpoint -> Host|1        |The signal when asserted by the endpoint informs that the transaction is started|
|`txn_cpl`   |Endpoint -> Host|1        |The signal when asserted by the endpoint informs that the transaction is completed|

**Table 2. :** `TXN` Group

### Channel Group

| **Signal** | **Direction** | **Size** | **Description** |
|:----------:|:-------------:|:--------:|:---------------:|
|`addr`      |Host -> Endpoint|N: 8 to 64|This is the address for the endpoint |
|`data`      |Host <-> Endpoint|N: {8, 16, 32, 64, 128, 256, 512, 1024}|The is the data signal, depending on where it is used, it can be input or output|

**Table 3. :** Channel Group

### Timing Diagram
![Base MX Bus Example](../img/mx_bus.png "MX Bus Timing Diagram")

## MX Read Bus
The read bus is designed to read data from the endpoint. The channel of the read bus contains the read address, `rd_addr`, and read data, `rd_data`, which is read from the endpoint.

### Read Channel

| **Signal** | **Direction** | **Size** | **Description** |
|:----------:|:-------------:|:--------:|:---------------:|
|`rd_addr`   |Host -> Endpoint|N: 8 to 64|This is the read address for the endpoint |
|`rd_data`   |Host <- Endpoint|N: {8, 16, 32, 64, 128, 256, 512, 1024}|The is the data signal, from the endpoint|

**Table 4. :** Read Channel

### Timing Diagram
![MX Read Bus](../img/read_mx_bus.png "MX Read Bus Timing Diagram")

## MX Write Bus
The write bus is designed to write data to the endpoint. The channel of the write bus contains the write address, `wr_addr`, and write data, `wr_data`, which is written to the endpoint.

### Write Channel

| **Signal** | **Direction** | **Size** | **Description** |
|:----------:|:-------------:|:--------:|:---------------:|
|`wr_addr`   |Host -> Endpoint|N: 8 to 64|This is the write address for the endpoint |
|`wr_data`   |Host -> Endpoint|N: {8, 16, 32, 64, 128, 256, 512, 1024}|The is the data signal, to the endpoint|

**Table 5. :** Write Channel

### Timing Diagram
![MX Write Bus](../img/write_mx_bus.png "MX Write Bus Timing Diagram")
