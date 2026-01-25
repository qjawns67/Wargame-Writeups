## Information
- Arch: x86-64, PE
- Tools: IDA

```c
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char v4[256]; // [rsp+20h] [rbp-118h] BYREF

  memset(v4, 0, sizeof(v4));
  sub_140001120("Input : ", argv, envp);
  sub_1400011B0("%256s", v4);
  if ( (unsigned int)sub_140001000((__int64)v4) )
    puts("Correct");
  else
    puts("Wrong");
  return 0;
}
```

```c
__int64 __fastcall sub_140001000(__int64 a1)
{
  int i; // [rsp+0h] [rbp-18h]

  for ( i = 0; (unsigned __int64)i < 31; ++i )
  {
    if ( (i ^ (unsigned __int8)__ROL1__(*(_BYTE *)(a1 + i), i & 7)) != byte_140003000[i] )
      return 0LL;
  }
  return 1LL;
}
```
## Analysis
i: 0~30, repeats 31 times
ROL1: rotation left

<img width="469" height="37" alt="image" src="https://github.com/user-attachments/assets/9d485207-6c5e-4cae-a0cb-9ae699eb71af" />

byte_140003000[i] information

Logic
- i ^ Rotation Left(a1, (i & 7)) == data[i]
- Rotation Left(a1, (i & 7)) == i ^ data[i]
- a1 == Rotation Right((i ^ data[i]), (i & 7))

So we need to do reverse opertation.
## Solution
```python
def ROR(a,b):
    right_shift=a >> b
    tmp=(a & 0xff) << 8-b

    return (right_shift | tmp) & 0xff

data=bytes.fromhex("52 DF B3 60 F1 8B 1C B5 57 D1 9F 38 4B 29 D9 26 7F C9 A3 E9 53 18 4F B8 6A CB 87 58 5B 39 1E 00")
data=data[:31]

final=[]

for i in range(31):
    final.append(ROR(i ^ data[i], i & 7))
    print(chr(ROR(i ^ data[i], i & 7)),end='')
```
