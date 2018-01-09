# Shake-related block


## Memory location
  
- Start address + 0x9E9BDC for the Japanese version
- Start address + 0x9E9C7C for the North American version, English language AND the European version, English language
  - Other languages haven't been tested much, but French might be +0x20 from English.


## Block items

Item | Offset | Bytes | Format | Notes
--- | --- | --- | --- | ---
Always 1.0? | 0x8 | 4 | Float
Wiimote shake bit/main | 0xC | 1 | Bit | [Spin related values](spin_related.md)
Wiimote continuous shake time/main | 0x10 | 4 | Integer | [Spin related values](spin_related.md)
Wiimote shake downtime/main | 0x14 | 4 | Integer | [Spin related values](spin_related.md)
Wiimote shake bit/water spins | 0x20 | 1 | Bit | [Spin related values](spin_related.md)
Wiimote continuous shake time/water spins | 0x24 | 4 | Integer | [Spin related values](spin_related.md)
Wiimote shake downtime/water spins | 0x28 | 4 | Integer | [Spin related values](spin_related.md)


## Possibly related memory locations

0x9EB3C0 (JP): This is another possible address used by the instruction that accesses "Wiimote shake downtime/main".
