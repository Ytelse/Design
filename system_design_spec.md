This document have some information about the system design architecture drawing. Document includes the following:

- System components
- Data rates of the Bus interfaces on the card.
- Calculations

# System components 

- SRAM: Block RAM should be enough to contain the entire network. If this isn't the case, an additional 512kB to 1MB SRAM chip should be sufficient.


# Data rates of bus interfaces

- Int 1: Supports max 12 Mbit/s (EFM32GG-RM p.242)
- Int 2: Max ~3.5 Mbit/s
  - Peripheral clock / 8 (EFM32GG-RM p.495)
  - Peripheral clock is max 28MHz (EFM32GG-RM p.126)
- Int 5: Between 10 MB/s and 50 MB/s depening on interface and protocol
- Int 7: Max theoretical 768 MB/s, estimating real capacity at 76.8 MB/s ( depending on CPU load and size of transfers )



# Calculations

## Ram usage

There are 784 input neurons. With 255 neurons in three layers, the following formula gives the total ammount of parameters for the first layer:

    784*255 + 255*255 + 255*255 + 255*10 = 332520 params.

For the second layer, the following formula gives the number of parameters:

    784*1024 + 1024*128 + 128*10 = 935168 params.

Some pruning can be done on the network. According to [1], the network can be pruned down by a factor of 40. Even in this optimistic case there will be 935168 / 40 = 23379 parameters. Given that the FPGA supports 30000 - 60000 [2], it is opptimistic to assume that the whole network can be implemented in logical blocks only. Some extra memory is required. The FPGA supports 1.8Mbit block RAM. This should be enough to contain the entire network. 

[1] Han, Song, Huizi Mao, and William J. Dally. "Deep compression: Compressing deep neural network with pruning, trained quantization and huffman coding." | CoRR,  abs/1510.001492 (2015).

[2] http://www.digikey.no/en/product-highlight/d/digilent/basys3-artix-7-fpga-board?WT.srch=1&mkwid=sLaWsedzG&pcrid=76350071888&pkw=_cat%3Adigikey.no&pmt=b&pdv=c
