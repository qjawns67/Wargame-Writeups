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
  if ( (unsigned int)sub_140001000(v4) )
    puts("Correct");
  else
    puts("Wrong");
  return 0;
}
```
## Analysis


## Solution
