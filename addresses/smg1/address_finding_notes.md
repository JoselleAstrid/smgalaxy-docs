# Address finding notes

Here are some notes/logs, sometimes long-winded, on how some of the addresses were found or were attempted to be found.


## Cheat Engine Tips
  
* In the Memory View window, right-click an instruction that get executed before some other instructions you're interested in. Select 'Break and trace'. Then let the game run so that the instruction gets run. In the trace results, you'll see a list of instructions that get run, and you can click on an instruction to see register values at the time it was run.

* Right-click an instruction and select "Toggle breakpoint" to set/unset a breakpoint on that instruction. In the Memory View window's menu, you can also see the list of breakpoints you've set. After a breakpoint is hit, use F7 to step, F9 to run, etc. (see the menu for more controls).

* Right-click an instruction and select "Set/Unset bookmark" if you just want to keep track of some instructions of interest.


## Shake related block's address

* Testing environment: JP version, Dolphin 5.0-3967. At Observatory in a completed file, as Luigi, to the left of the start, on the steps leading to the green launch star. (3rd step from the top, I think.) Standing still and haven't done anything for a few seconds.

* Two instructions access shake downtime/main (location 0x9E9BF0) on each frame. They do so with address `rbx+rsi+14`, where `rbx` is 0x27FFF0000 and `rsi` is 0x809E9BDC.

* The first such instruction is `mov edi,[rbx+rsi+14]`. We'll call this instruction accessor1.

* accessor1 is reached 4 times per frame (again, when M/L haven't done anything recently). `rsi` is different each time. The FIRST time accessor1 is reached, `rsi` is 0x809E9BDC.

* For reference, the closest `ret` we find when backpedaling from accessor1 is found at accessor1-0x941. After that `ret` is a `jg` which is at accessor1-0x940. We'll call this `jg` func1.

* There is a long, mostly sequential sequence which starts at a `jg` after a `ret` and a few `int 3`s, and makes its way all the way to func1. Let's call this `jg` func2. func2 is at func1-0x5B48. Most of the code from func2 to func1 can be stepped through sanely, except:
    
  * At around func2+0x13xx to +0x19xx there is a big loop which might run like 120 times (till RAX wraps from all Fs to 0).
  * Same at around +0x35xx to +0x44xx.
  * Similar at around +0x43xx to +0x46xx, except:
    * It's about 30 times, not 120
    * The jump back actually goes like -0x31xx, but it ends up returning back to about -0x3xx
    * The loop-ending jump is NOT the `jnl` near the backwards jump; instead, it's a `jmp` about 0x80 earlier.
  
* We MAY reach (or skip) a `call` at func1-0x226, which goes to a `jg` at func1-0xE8690. We'll call this `jg` func3.
    
* Notable jumps/returns from func2 to func1:

  Address | Instruction | Destination
  --- | --- | ---
  func1-0x14F6 | jmp | func1-0x1340
  func1-0x1255 | ret | func1-0x1D20
  func1-0x1D18 | jmp | func1-0x1254
  func1-0xD8A | ret | func2+0x607
  func2+0x60F | jmp | func1-0xD88
  func1-0xCBB | jmp | func1-0xC64
  func1-0xB90 | jmp | func1-0x9A4
  func1-0x876 | jmp | func1-0x720

* Instructions that determine what accessor1 reads, in forwards order, leading up to the FIRST time accessor1 is reached:

  * func3 route (this table may be incomplete):

    Address | Instruction | Registers | RAM values
    --- | --- | --- | ---
    func3+0xE | `mov esi,[rbp-54]` | `rbp` = 0x1806AE0 | `[rbp-54]` = 0x38DF6B80
    func3+0x41 | `lea edi,[rsi-04]`| `rsi` = 0x806BDED8
    func3+0x44 | `mov edi,[rbx+rdi+00]` | `rbx` = 0x27FFF0000
    func3+0x48 | `bswap edi`
    func3+0x4A | `mov [rbp-04],edi`
     | `ret` (to func1-0x226)
    func1-0x219 | `jmp` (to a `jg` at func1+0x75A0)
     | `ret` (to func1-0xD51)
    func1-0xD44 | `jmp` (to func1)
  
  * Non-func3 route:
    
    Address | Instruction | Registers | RAM values
    --- | --- | --- | ---
    func1-0x1318 | `mov esi,[rbp-7C]` | `rsi` = 0x806BDE68
    func1-0x1277 | `add esi,50` | `rsi` = 0x806BDEB8
    func1-0x1274 | `mov [rbp-7C],esi` | `rbp` = 0x1806AE0 | `[rbp-7C]` = 0x806BDEB8
    func1-0x121F | `mov r13d,[rbp-7C]` | `r13` = 0x806BDEB8
    func1-0x1211 | `add r13d,-20` | `r13` = 0x806BDE98
    func1-0x11B5 | `add r13d,-10` | `r13` = 0x806BDE88
    func1-0x117F | `mov [rbp-7C],r13d` | `[rbp-7C]` = 0x806BDE88
    func1-0x10FC1E (called from func1-0x11D1) | `mov r13d,[rbp-7C]` | `r13` = 0x806BDE88
    func1-0x10FBF6 (called from func1-0x11D1) | `add r13d,10` | `r13` = 0x806BDE98
    func1-0x10FBF2 (called from func1-0x11D1) | `mov [rbp-7C],r13d` | `[rbp-7C]` = 0x806BDE98
    func1-0xE5E | `mov esi,[rbp-7C]` | `rsi` = 0x806BDE98
    func1-0xE40 | `add esi,20` | `rsi` = 0x806BDEB8
    func1-0xE3D | `mov [rbp-7C],esi` | - | `[rbp-7C]` = 0x806BDEB8
    func1-0xE0E | `mov esi,[rbp-7C]` | `rsi` = 0x806BDEB8
    func1-0xDAC | `add esi,50` | `rsi` = 0x806BDF08
    func1-0xDA9 | `mov [rbp-7C],esi` | - | `[rbp-7C]` = 0x806BDF08
    func1-0xD3F | `mov edi,[rbp-7C]` | `rdi` = 0x806BDF08
    func1-0xD34 | `add edi,-30` | `rdi` = 0x806BDED8
    func1-0xCDB | `mov [rbp-7C],edi` | `[rbp-7C]` = 0x806BDED8
    func1-0x29A | `jmp` (to func1-0x214)
     | `jmp` (to func1-0xEC)
    func1-0xC4 | `mov esi,[rbp-7C]` | `rsi` = 0x806BDED8
    func1-0xAB | `lea edi,[rsi+20]` | `rdi` = 0x806BDEF8
    func1-0x42 | `lea r13d,[rdi-04]` | `r13` = 0x806BDEF4
    func1-0x3E | `mov r13d,[rbx+r13+00]` | `r13` = 0xF08C9E80
    func1-0x39 | `bswap r13d` | `r13` = 0x809E8CF0
    func1-0x36 | `mov [rbp-04],r13d` | - | `[rbp-04]` = 0x809E8CF0
    func1-0x1 | `ret` (to a `pop` at func1-0xD4C)
     | `jmp` (to func1)
    
  * Routes merge; after func1 is reached:
  
    Address | Instruction | Registers | RAM values
    --- | --- | --- | ---
    func1+0xE | `mov esi,[rbp-04]` | `rsi` = 0x809E8CF0
    func1+0x11 | `mov edi,[rbx+rsi+14]` | `rdi` = 0xDC9B9E80
    func1+0x15 | `bswap edi` | `rdi` = 0x809E9BDC
    func1+0xC8 | `mov esi,edi` | `rsi` = 0x809E9BDC
    func1+0xDC | `mov [rbp-04],esi` | - | `[rbp-04]` = 0x809E9BDC
    accessor1-0x32E | `jmp` (to accessor1-0xB8)
    accessor1-0x9D | `mov esi,[rbp-04]` | `rbp` = 0x1806AE0, `rsi` = 0x809E9BDC
    accessor1 | `mov edi,[rbx+rsi+14]` | `rbx` = start-0x80000000
    
  One possible formula is the following, based on the non-func3 route:
  
  * Shake downtime/main = `[rbx + (rsi) + 14]`
  * = `[rbx + [rbx+rsi+14] + 14]`
  * = `[rbx + [rbx+[rbp-04]+14] + 14]`
  * = `[rbx + [rbx+[rbx+r13+00]+14] + 14]`
  * = `[rbx + [rbx+[rbx+rdi-04]+14] + 14]`
  * = `[rbx + [rbx+[rbx+rsi+1C]+14] + 14]`
  * = `[rbx + [rbx+[rbx+[rbp-7C]+1C]+14] + 14]`
  * = `[rbx + [rbx+[rbx+0x806BDED8+1C]+14] + 14]`
  
  But `[rbx+0x806BDED8+1C]` is modified by over 20 different instructions per frame. Not reliable for our purposes at all.
  
  The formula `[rbx + [rbx+0x809E8CF0+14] + 14]` appears to work for JP, and does not null out when entering/exiting a dome. But it doesn't work for NA. It does work for NA if we change 0x809E8CF0 to 0x809E8D90.
  
  However, at this point we may as well use the more direct address 0x809E9BDC, which also requires a +0xA0 adjustment going from JP to NA.
  
  A version-agnostic formula would've been nice, but it seems that will require even more digging to find.
