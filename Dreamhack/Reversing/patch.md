## Information
- File: x86-64, WinAPI GUI Program
- Tools: IDA

<img width="586" height="193" alt="image" src="https://github.com/user-attachments/assets/027ad3ea-4c8a-4b85-82fd-6f3763dde067" />  

We should analyze the logic of drawing flag, and remove things covering the flag.

## Analysis
1. This is GUI Program of WinAPI, we should find WinMain()
```c
v11.lpfnWndProc = (WNDPROC)sub_1400032F0;
```
2. We need to focus on subroutines made by developers like that
```c
LRESULT __fastcall sub_1400032F0(HWND a1, UINT a2, WPARAM a3, LPARAM a4)
{
  _QWORD *v5; // rbx
  __int64 v6; // rbx
  __int64 v7; // [rsp+20h] [rbp-18h] BYREF

  switch ( a2 )
  {
    case 2u:
      PostQuitMessage(0);
      return 0LL;
    case 0xFu:
      qword_140007910 = (__int64)BeginPaint(hWnd, &Paint);
      v5 = (_QWORD *)GdipAlloc(16LL);
      if ( v5 )
      {
        *v5 = 0LL;
        v5[1] = 0LL;
        v7 = 0LL;
        *((_DWORD *)v5 + 2) = GdipCreateFromHDC(qword_140007910, &v7);
        *v5 = v7;
      }
      else
      {
        v5 = 0LL;
      }
      qword_140007918 = (__int64)v5;
      sub_140002C40();
      v6 = qword_140007918;
      if ( qword_140007918 )
      {
        GdipDeleteGraphics(*(_QWORD *)qword_140007918);
        GdipFree(v6);
      }
      EndPaint(hWnd, &Paint);
      return 0LL;
    case 0x202u:
      InvalidateRect(hWnd, 0LL, 1);
      UpdateWindow(hWnd);
      return 0LL;
    default:
      return DefWindowProcW(a1, a2, a3, a4);
  }
}
```
3. There is a switch-case, 2u: Post Quit Messages / 0xFu: Paint things? / 0x202u: Update Window
   So we need to focus on sub_140002C40() subroutine between BeginPaint and EndPaint.

```c
char __fastcall sub_140002C40(__int64 a1, int a2)
{
  int v2; // ebx
  int v3; // edx
  int v4; // edx
  int v5; // edx
  int v6; // edx
  int v7; // edx
  int v8; // edx
  int v9; // edx
  int v10; // edx
  int v11; // edx
  int v12; // edx
  int v13; // edx
  int v14; // edx
  int v15; // edx
  int v16; // edx
  int v17; // edx
  int v18; // edx
  int v19; // edx
  int v20; // edx
  int v21; // edx
  int v22; // edx
  int v23; // edx
  int v24; // edx
  int v25; // edx
  int v26; // edx
  __int64 v27; // rbx
  __int64 v28; // r8
  __int64 v29; // r8
  __int64 v30; // rdx
  __int64 v31; // r8
  __int64 v32; // rdx
  __int64 v33; // r8
  __int64 v34; // rdx
  __int64 v35; // r8
  __int64 v36; // rdx
  __int64 v37; // r8
  __int64 v38; // rdx
  __int64 v39; // r8
  __int64 v40; // rdx
  __int64 v41; // r8
  __int64 v42; // r8
  __int64 v43; // rdx
  __int64 v44; // r8
  __int64 v45; // r8
  __int64 v46; // rdx
  __int64 v47; // r8

  v2 = qword_140007880;
  sub_140002B80(qword_140007880, a2, 30, 470, 80, -16777216);
  sub_140002B80(v2, v3, 35, 470, 75, -16777216);
  sub_140002B80(v2, v4, 40, 470, 70, -16777216);
  sub_140002B80(v2, v5, 45, 470, 65, -16777216);
  sub_140002B80(v2, v6, 50, 470, 60, -16777216);
  sub_140002B80(v2, v7, 55, 470, 55, -16777216);
  sub_140002B80(v2, v8, 60, 470, 50, -16777216);
  sub_140002B80(v2, v9, 65, 470, 45, -16777216);
  sub_140002B80(v2, v10, 70, 470, 40, -16777216);
  sub_140002B80(v2, v11, 75, 470, 75, -16777216);
  sub_140002B80(v2, v12, 80, 400, 60, -16777216);
  sub_140002B80(v2, v13, 30, 470, 90, -16777216);
  sub_140002B80(v2, v14, 35, 470, 30, -16777216);
  sub_140002B80(v2, v15, 40, 470, 35, -16777216);
  sub_140002B80(v2, v16, 45, 470, 50, -16777216);
  sub_140002B80(v2, v17, 50, 470, 40, -16777216);
  sub_140002B80(v2, v18, 55, 400, 90, -16777216);
  sub_140002B80(v2, v19, 60, 470, 60, -16777216);
  sub_140002B80(v2, v20, 65, 470, 30, -16777216);
  sub_140002B80(v2, v21, 70, 470, 80, -16777216);
  sub_140002B80(v2, v22, 75, 470, 70, -16777216);
  sub_140002B80(v2, v23, 80, 470, 60, -16777216);
  sub_140002B80(v2, v24, 80, 470, 80, -16777216);
  sub_140002B80(v2, v25, 80, 470, 70, -16777216);
  sub_140002B80(v2, v26, 90, 470, 90, -16777216);
  v27 = qword_140007880;
  sub_1400017A0(qword_140007880, 40LL, v28, 4278190080LL);
  sub_140001C80(v27, 80LL, v29, 4278190080LL);
  sub_140002640(v27, v30, v31, 4278190080LL);
  sub_1400020F0(v27, v32, v33, 4278190080LL);
  sub_140002390(v27, v34, v35, 4278190080LL);
  sub_140001240(v27, v36, v37, 4278190080LL);
  sub_140001F20(v27, v38, v39, 4278190080LL);
  sub_140001560(v27, v40, v41, 4278190080LL);
  sub_140001C80(v27, 360LL, v42, 4278190080LL);
  sub_1400019D0(v27, v43, v44, 4278190080LL);
  sub_1400017A0(v27, 440LL, v45, 4278190080LL);
  sub_140002870(v27, v46, v47, 4278190080LL);
  return 0;
}
```
4. There is a lot of 2B80 sub, and little bit of other subroutine.
   So we need to analyze both
```c
__int64 __fastcall sub_140002B80(__int64 a1, __int64 a2, unsigned int a3, int a4, int a5, unsigned int a6)
{
  int v9; // eax
  __int64 v10; // rsi
  int v11; // eax
  __int64 v12; // rbx
  int v13; // eax
  __int64 v15; // [rsp+30h] [rbp-38h] BYREF
  __int64 v16; // [rsp+38h] [rbp-30h]

  v16 = 0LL;
  v15 = 0LL;
  v9 = GdipCreatePen1(a6, a2, 0LL, &v15);
  v10 = *(_QWORD *)(a1 + 120);
  LODWORD(v16) = v9;
  v11 = GdipDrawLineI(*(_QWORD *)v10, v15, 150LL, a3, a4, a5);
  if ( v11 )
    *(_DWORD *)(v10 + 8) = v11;
  v12 = *(_QWORD *)(a1 + 120);
  v13 = GdipSetSmoothingMode(*(_QWORD *)v12, 4LL);
  if ( v13 )
    *(_DWORD *)(v12 + 8) = v13;
  return GdipDeletePen(v15);
}
```
```c
double __fastcall sub_1400017A0(__int64 a1, __int64 a2, __int64 a3, unsigned int a4)
{
  unsigned int v5; // r14d
  __int64 v7; // rsi
  int v8; // eax
  __int64 v9; // rsi
  int v10; // eax
  __int64 v11; // rdx
  __int64 v12; // rsi
  int v13; // eax
  __int64 v14; // rsi
  int v15; // eax
  __int64 v16; // rdx
  __int64 v17; // rsi
  int v18; // eax
  __int64 v19; // rsi
  int v20; // eax
  __int64 v21; // rdx
  __int64 v22; // rbx
  int v23; // eax
  __int64 v24; // rbx
  int v25; // eax
  __int64 v27; // [rsp+38h] [rbp-28h] BYREF
  __int64 v28; // [rsp+40h] [rbp-20h]

  v5 = a2;
  v28 = 0LL;
  v27 = 0LL;
  LODWORD(v28) = GdipCreatePen1(a4, a2, 0LL, &v27);
  v7 = *(_QWORD *)(a1 + 120);
  v8 = GdipDrawLineI(*(_QWORD *)v7, v27, v5, 31LL, v5, 59);
  if ( v8 )
    *(_DWORD *)(v7 + 8) = v8;
  v9 = *(_QWORD *)(a1 + 120);
  v10 = GdipSetSmoothingMode(*(_QWORD *)v9, 4LL);
  if ( v10 )
    *(_DWORD *)(v9 + 8) = v10;
  GdipDeletePen(v27);
  v28 = 0LL;
  v27 = 0LL;
  LODWORD(v28) = GdipCreatePen1(a4, v11, 0LL, &v27);
  v12 = *(_QWORD *)(a1 + 120);
  v13 = GdipDrawLineI(*(_QWORD *)v12, v27, v5, 31LL, v5 + 28, 59);
  if ( v13 )
    *(_DWORD *)(v12 + 8) = v13;
  v14 = *(_QWORD *)(a1 + 120);
  v15 = GdipSetSmoothingMode(*(_QWORD *)v14, 4LL);
  if ( v15 )
    *(_DWORD *)(v14 + 8) = v15;
  GdipDeletePen(v27);
  v28 = 0LL;
  v27 = 0LL;
  LODWORD(v28) = GdipCreatePen1(a4, v16, 0LL, &v27);
  v17 = *(_QWORD *)(a1 + 120);
  v18 = GdipDrawLineI(*(_QWORD *)v17, v27, v5 + 28, 60LL, v5, 88);
  if ( v18 )
    *(_DWORD *)(v17 + 8) = v18;
  v19 = *(_QWORD *)(a1 + 120);
  v20 = GdipSetSmoothingMode(*(_QWORD *)v19, 4LL);
  if ( v20 )
    *(_DWORD *)(v19 + 8) = v20;
  GdipDeletePen(v27);
  v28 = 0LL;
  v27 = 0LL;
  LODWORD(v28) = GdipCreatePen1(a4, v21, 0LL, &v27);
  v22 = *(_QWORD *)(a1 + 120);
  v23 = GdipDrawLineI(*(_QWORD *)v22, v27, v5, 60LL, v5, 90);
  if ( v23 )
    *(_DWORD *)(v22 + 8) = v23;
  v24 = *(_QWORD *)(a1 + 120);
  v25 = GdipSetSmoothingMode(*(_QWORD *)v24, 4LL);
  if ( v25 )
    *(_DWORD *)(v24 + 8) = v25;
  return GdipDeletePen(v27);
}
```
5. 2B80 is drawing one line, any other sub is drawing a lot of lines, so we need debug to check what sub is drawing flag or not.

## Solution
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/6d7804f3-59fb-4ca2-a289-46bf7552ccc4" />

1. Set the breakpoint at the first 2B80.
   Countinuing Step Over, we can find out 2B80 is drawing covering, So we need to patch that binary by editing code.
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/26e02ffb-d2e9-4d77-88b9-0097a4254a1d" />

2. Edit - Patch Program - Assemble, and go to assembly of that sub, just input ret
   and then, Edit - Patch Program - Apply patches to input file.
<img width="586" height="193" alt="image" src="https://github.com/user-attachments/assets/c711c096-12c5-47df-b56b-c6d870a5598a" />

Then, we can find out the flag!


