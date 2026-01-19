## Information
- Arch: x86-64, PE
- Tools: IDA

## Analysis
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



## Solution
