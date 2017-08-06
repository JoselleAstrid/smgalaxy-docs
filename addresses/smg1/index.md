# Addresses

## Notes

- \[Brackets\] in an address calculation indicate a pointer dereference.
- "(Dupe)" means a value seems to be an exact duplicate of a previously covered value.
- "(A)" "(B)" etc. denote multiple versions of basically the same value. Either the nature of the differences is unknown, or it's not known whether there are differences.
- "(Derived)" means a value seems to have a simple derivation from a previously covered value, and it's unknown whether this value serves some other purpose.
- "-" for Item and Type means the value is always 0 from observations so far, or it's not always 0 but it's totally unknown what kind of value it could be.

## Base addresses and pointers

- Reference pointer offset:
  - 0xF8EF88 for the North American and PAL versions (tested with English language for both)
  - 0xF8F328 for the Japanese version
- Reference pointer = Start address + \[Start address + Reference pointer offset\] - 0x80000000

- Alternate reference pointer = Start address + \[Start address + 0x6B7B40\] - 0x80000000
  - This might work for both the North American and Japanese versions, but it needs more testing.
  - This reference pointer's value is 0x18D0 greater than the above reference pointer.
  - Source: "Infinite Health" code by dexter0

## Memory values

- [Static-address values](static.md)
- [Reference pointer based values](ref_based.md)
