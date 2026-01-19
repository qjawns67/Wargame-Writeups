## Information
- Arch: x86-64, PE
- Tools: IDA


```c
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char v4[256]; // [rsp+20h] [rbp-118h] BYREF

  memset(v4, 0, sizeof(v4));
  sub_1400011C0("Input : ", argv, envp);
  sub_140001220("%256s", v4);
  if ( (unsigned int)sub_140001000((__int64)v4) )
    puts("Correct");
  else
    puts("Wrong");
  return 0;
}
```
We should find out the correct string.
## Analysis
We need to find out the logic of the sub_140001000()
```c
__int64 __fastcall sub_140001000(__int64 a1)
{
  int i; // [rsp+0h] [rbp-18h]

  for ( i = 0; (unsigned __int64)i < 28; ++i )
  {
    if ( ((unsigned __int8)(16 * *(_BYTE *)(a1 + i)) | ((int)*(unsigned __int8 *)(a1 + i) >> 4)) != byte_140003000[i] )
      return 0LL;
  }
  return 1LL;
}
```
1. a1 argument is v4
2. for statement repeats 28 times

3-1. we should find out the byte_140003000[i]
<img width="706" height="68" alt="image" src="https://github.com/user-attachments/assets/883b4ca1-ccbe-42fd-b229-f383dbe8285e" />
<img width="468" height="33" alt="image" src="https://github.com/user-attachments/assets/cc6c3f42-ea1f-48d9-b4dc-59296de2b351" />
We can get a string of the bytes information.

3-2. ((unsigned __int8)(16 * *(_BYTE *)(a1 + i)) : multiplying 16(Decimal) means that left shift 4 times.    
3-3. ((int)*(unsigned __int8 *)(a1 + i) >> 4)): that means right shift 4 times.  
So we can find that if the word is 10010110, 3-2 is 01100000, 3-3 is 00001001, so result of OR, we can get 011010001.  
That is result of reversing sequence front side and back side.  

## Solution
We found the logic, so we can write the script for python.  
```python
data=bytes.fromhex("24 27 13 C6 C6 13 16 E6 47 F5 26 96 47 F5 46 27 13 26 26 C6 56 F5 C3 C3 F5 E3 E3 00 00 00 00 00")
data=data[:28] # Slicing ~28

for i in range(28):
    tmp=f"{data[i]:08b}"
    a=tmp[0:4]
    b=tmp[4:8]
    tmp=f"{b}{a}"
    print(chr(int(tmp,2)),end='')
```
