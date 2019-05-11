<!-- MarkdownTOC -->

- [1. 多字节字符串与宽字节字符串相互转换](#1-多字节字符串与宽字节字符串相互转换)

<!-- /MarkdownTOC -->


<a id="1-多字节字符串与宽字节字符串相互转换"></a>
#### 1. 多字节字符串与宽字节字符串相互转换
```
#define _CRT_SECURE_NO_WARNINGS
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
#include <Windows.h>
#include <locale.h>

int main(int argc, char* argv[])
{
    DWORD dwNum;

    setlocale(LC_ALL, "");
    
    char sText[] = "多字节转宽字节";

    dwNum = MultiByteToWideChar(CP_ACP, 0, sText, -1, NULL, 0);

    wchar_t *pwText;
    pwText = new wchar_t[dwNum];
    if (!pwText)
    {
        delete[] pwText;
        return 1;
    }

    MultiByteToWideChar(CP_ACP, 0, sText, -1, pwText, dwNum);
    wprintf(L"M->W: %s\n", pwText);
    delete[] pwText;

    wchar_t wText[] = L"宽字节字符串转多字节字符串";
    dwNum = WideCharToMultiByte(CP_OEMCP, 0, wText, -1, NULL, 0, NULL, NULL);

    char *psText;
    psText = new char[dwNum];
    if (!psText)
    {
        delete[] psText;
        return 2;
    }

    WideCharToMultiByte(CP_OEMCP, 0, wText, -1, psText, dwNum, NULL, NULL);
    printf("W->M: %s\n", psText);

    delete[] psText;

    system("pause");
    return 0;
}
result:
M->W: 多字节转宽字节
W->M: 宽字节字符串转多字节字符串
请按任意键继续. . .
```

