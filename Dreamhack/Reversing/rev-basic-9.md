## Information
- Arch: x86-64, PE
- Tools: IDA

<img width="828" height="323" alt="image" src="https://github.com/user-attachments/assets/49f5c62c-b3ff-4c70-8dbf-1c350c84e0e4" />

Decompiled Function main

<img width="626" height="291" alt="image" src="https://github.com/user-attachments/assets/b83c0a9d-34b5-4ea8-b8d5-64c5005a1a4e" />

Decompiled sub140001000()
- repeats for statement, and executing sub_1400010A0(), input data is a1, v4
- And finally through memcmp(), a1 is the same as the unk_140004000 for returning True

<img width="471" height="29" alt="image" src="https://github.com/user-attachments/assets/cd7621b8-7627-4701-909c-a5f58ab984d7" />

Hex View for 140004000 data

```c
__int64 __fastcall sub_1400010A0(unsigned __int8 *a1)
{
  __int64 result; // rax
  unsigned __int8 v2; // [rsp+0h] [rbp-48h]
  int j; // [rsp+4h] [rbp-44h]
  int i; // [rsp+8h] [rbp-40h]
  char v5[16]; // [rsp+10h] [rbp-38h] BYREF

  strcpy(v5, "I_am_KEY");
  result = *a1;
  v2 = *a1;
  for ( i = 0; i < 16; ++i )
  {
    for ( j = 0; j < 8; ++j )
    {
      v2 = __ROR1__(a1[((_BYTE)j + 1) & 7] + byte_140004020[(unsigned __int8)v5[j] ^ v2], 5);
      a1[((_BYTE)j + 1) & 7] = v2;
    }
    result = (unsigned int)(i + 1);
  }
  return result;
}
```
Decompiled sub_1400010A0()  
- v5 is just bytes of "I_am_KEY:  
- We can figure out that function is changing value of a1 through various operations  
- So, we need to find out correct value of a1, using this logic  

## Analysis

We need to find out reverse operation.  

a1[((_BYTE)j + 1) & 7] = v2;  
-> v2 = a1[(j + 1) & 7]  

v2 = ROR1(a1[((_BYTE)j + 1) & 7] + byte_140004020[(unsigned __int8)v5[j] ^ v2], 5);  
-> ROL(v2, 5) = a1[(j + 1) & 7] + byte_140004020[(unsigned __int8)v5[j] ^ v2]  
-> a1[(j + 1) & 7] = ROL(v2, 5) - byte_140004020[v5[j] ^ v2]  

## Solution

```python
def ROL(a,b):
    left_shift=a << b
    tmp=(a & 0xff) >> 8-b

    return (left_shift | tmp) & 0xff

data4020=bytes.fromhex("63 7C 77 7B F2 6B 6F C5 30 01 67 2B FE D7 AB 76 CA 82 C9 7D FA 59 47 F0 AD D4 A2 AF 9C A4 72 C0 B7 FD 93 26 36 3F F7 CC 34 A5 E5 F1 71 D8 31 15 04 C7 23 C3 18 96 05 9A 07 12 80 E2 EB 27 B2 75 09 83 2C 1A 1B 6E 5A A0 52 3B D6 B3 29 E3 2F 84 53 D1 00 ED 20 FC B1 5B 6A CB BE 39 4A 4C 58 CF D0 EF AA FB 43 4D 33 85 45 F9 02 7F 50 3C 9F A8 51 A3 40 8F 92 9D 38 F5 BC B6 DA 21 10 FF F3 D2 CD 0C 13 EC 5F 97 44 17 C4 A7 7E 3D 64 5D 19 73 60 81 4F DC 22 2A 90 88 46 EE B8 14 DE 5E 0B DB E0 32 3A 0A 49 06 24 5C C2 D3 AC 62 91 95 E4 79 E7 C8 37 6D 8D D5 4E A9 6C 56 F4 EA 65 7A AE 08 BA 78 25 2E 1C A6 B4 C6 E8 DD 74 1F 4B BD 8B 8A 70 3E B5 66 48 03 F6 0E 61 35 57 B9 86 C1 1D 9E E1 F8 98 11 69 D9 8E 94 9B 1E 87 E9 CE 55 28 DF 8C A1 89 0D BF E6 42 68 41 99 2D 0F B0 54 BB 16")

data4000=bytes.fromhex("2A 79 00 7D C4 2A 4F 58")
#7E 7D 9A 8B 25 2D D5 3D / 03 2B 38 98 27 9F 4F BC / 2A 79 00 7D C4 2A 4F 58 00 00 00 00 00 00 00 00")
data4000=data4000[:25]

a1=bytearray(data4000)
v5=bytearray(b"I_am_KEY")


for i in range(16):
    for j in range(8):
        v2 = a1[(7-j+1) & 7]
        a1[(7-j+1) & 7] = (ROL(v2,5)-data4020[v5[7-j]^a1[(7-j)&7]]) & 0xff

print(a1.hex().upper())
```
- I divided a1 for 1 byte, and execute three times
- First, we should make sure v2 value for using 140004000 data, and then, make sure a1 value
- 0 ~ 15, 0 ~ 7, all the for statement should be reversed, 15 ~ 0, 7 ~ 0.
- And when doing the XOR decryption, we need a previous a1 value, so a1[(7-j)&7] is right.
