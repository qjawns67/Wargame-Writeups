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
We should find out correct string.  
## Analysis
```c
__int64 __fastcall sub_140001000(__int64 a1)
{
  int i; // [rsp+0h] [rbp-18h]

  for ( i = 0; (unsigned __int64)i < 24; ++i )
  {
    if ( *(unsigned __int8 *)(a1 + i + 1) + *(unsigned __int8 *)(a1 + i) != byte_140003000[i] )
      return 0LL;
  }
  return 1LL;
}
```
First, we should find out 140003000, and then we find out our input value  
<img width="469" height="37" alt="image" src="https://github.com/user-attachments/assets/f2b20569-e11f-4bed-a505-7f4c7a4f7b2c" />

We can get that easily.  

Now, we should know about *(unsigned __int8 *)(a1 + i + 1) + *(unsigned __int8 *)(a1 + i) logic.  
That means if i=0, int8 (a1+1) + int8 a1 = 3000[0].  
So expanding at the end, (a1+24) + (a1+23) = 3000[23] -> a1+23 == 0  
So we can get other char as sequencing from the end.  

## Solution
```python
data=bytes.fromhex("AD D8 CB CB 9D 97 CB C4 92 A1 D2 D7 D2 D6 A8 A5 DC C7 AD A3 A1 98 4C 00 00 00 00 00 00 00 00 00")
data=data[:24] # Slicing ~24

final=[0] # 

for i in range(23):
    tmp=data[22-i]-int(final[i]) # From the end
    final.append(tmp)

final=final[::-1] # Reverse

for i in range(23):
    print(chr(final[i]),end='')
```
