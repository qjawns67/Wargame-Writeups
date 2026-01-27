## Exploit
```python
def XORWithParam2(param_1, param_2):
    sVar1=len(param_2)
    for i in range(32):
        param_1[i]=(param_2[i%sVar1] ^ param_1[i]) & 0xff


def IncWithParam2(param_1, param_2):
    for i in range(32):
        param_1[i] = (param_1[i] + param_2) &0xff 

def DecWithParam2(param_1, param_2):
    for i in range(32):
        param_1[i] = (param_1[i] - param_2) &0xff

def Decode(encoded):
    XORWithParam2(encoded,bytes.fromhex("11 33 55 77 99 bb dd"))
    DecWithParam2(encoded, -13)
    IncWithParam2(encoded, 77)
    XORWithParam2(encoded,bytes.fromhex("ef be ad de"))
    IncWithParam2(encoded, 90)
    DecWithParam2(encoded, 31)
    XORWithParam2(encoded, bytes.fromhex("de ad be ef"))

data=bytes.fromhex("f8 e0 e6 9e 7f 32 68 31 05 dc a1 aa aa 09 b3 d8 41 f0 36 8c ce c7 ac 66 91 4c 32 ff 05 e0 d9 91")
data_list=list(data)

Decode(data_list)

print("Decoded Hex:", bytes(data_list).hex(' '))
print("Decoded String:", "".join(map(chr, data_list)))
```

Put &0xff for using byte type.
I need to write code through C for easier coding. 
