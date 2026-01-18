## Information
- Arch: x86-64
- Tools: IDA

<img width="347" height="156" alt="image" src="https://github.com/user-attachments/assets/c25dda57-73d3-4b30-b9c7-92df9d456f42" />
<img width="487" height="198" alt="image" src="https://github.com/user-attachments/assets/514ff4c1-5218-4535-8887-446d21255b4b" />


We should find out correct string.
## Analysis
```c
__int64 __fastcall sub_140001000(__int64 a1)
{
  int i; // [rsp+0h] [rbp-18h]

  for ( i = 0; (unsigned __int64)i < 24; ++i )
  {
    if ( byte_140003000[i] != (i ^ *(unsigned __int8 *)(a1 + i)) + 2 * i )
      return 0LL;
  }
  return 1LL;
}
```
we should know the logic of sub_140001000()
1. i: 0~23, 24 times repeat
2. a1, argument is v4, our input string
3. We should find out string that matches byte_140003000[i], So we can find the value because already stored.

<img width="729" height="58" alt="image" src="https://github.com/user-attachments/assets/c8dc3dea-1f96-420a-96fb-fd8e0f600633" />
<img width="601" height="35" alt="image" src="https://github.com/user-attachments/assets/d45386ac-7447-4b97-92ae-b486c54eea73" />

We can view that value for Hex View, (and we can export that value for .txt(Shift+E))

Next, we got a 3000[i], and then we should decryption value one by one.
So we need to code python script.

## Solution

```python
data=bytes.fromhex("49 60 67 74 63 67 42 66 80 78 69 69 7B 99 6D 88 68 94 9F 8D 4D A5 9D 45 00 00 00 00 00 00 00 0049 60 67 74 63 67 42 66 80 78 69 69 7B 99 6D 88 68 94 9F 8D 4D A5 9D 45 00 00 00 00 00 00 00 00")
data=data[:24] # Slicing ~24

result=[]

for i in range(24):
    tmp=data[i]-2*i
    tmp=tmp%256 # int8 (0~255), Overflow alert!
    a1=tmp^i # XOR Logic

    result.append(a1)

result=bytes(result) #For printing char

print(result)
```
```
b'I_am_X0_xo_Xor_eXcit1ng\x00'
```
We can get strings like that. b'' means bytes, \x00 is 00h, NULL string
