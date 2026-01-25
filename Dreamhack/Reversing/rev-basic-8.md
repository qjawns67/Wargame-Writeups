## Information
- Arch: x86-64, PE
- Tools: IDA

```c
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char v4[256]; // [rsp+20h] [rbp-118h] BYREF

  memset(v4, 0, sizeof(v4));
  sub_1400011B0("Input : ", argv, envp);
  sub_140001210("%256s", v4);
  if ( (unsigned int)sub_140001000(v4) )
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

  for ( i = 0; (unsigned __int64)i < 0x15; ++i )
  {
    if ( (unsigned __int8)(-5 * *(_BYTE *)(a1 + i)) != byte_140003000[i] )
      return 0LL;
  }
  return 1LL;
}
```

## Analysis

What we need to know

1. When combined int and byte, it promoted to Integer
2. In this case of int8, -5 is the same as 251.
3. We can't do reverse operation simply, we should use modular multiplicative inverse

Modular Multiplicative Inverse?

1. We need to find out modular inverse: pow(target, -1, modular) -> modular inverse of -5, 251 is 51
2. Our logic is -5 * X == data[i], so we can multiply modular inverse of -5.  
   Then, X == data[i] * 51, so we can get a target value without brute-force.  
   

## Solution
```python
data=bytes.fromhex("AC F3 0C 25 A3 10 B7 25 16 C6 B7 BC 07 25 02 D5 C6 11 07 C5 00 00 00 00 00 00 00 00 00 00 00 00")
data=data[:21] # Slicing ~21

final=[]

for i in range(21):
    for j in range(0,256):
        tmp=(251*j)%256

        if tmp==int(data[i]):
            print(chr(j),end='')
            break;
```
1. Just Brute-force way

```python
data=bytes.fromhex("AC F3 0C 25 A3 10 B7 25 16 C6 B7 BC 07 25 02 D5 C6 11 07 C5 00 00 00 00 00 00 00 00 00 00 00 00")
data=data[:21] # Slicing ~21

final=[]
modular_inverse=pow(251,-1,256)
print(f"Inverse of -5 is {modular_inverse}")

for i in range(21):
    tmp=data[i]*modular_inverse%256
    print(chr(tmp),end='')
```
2. Efficient way through modular inverse
