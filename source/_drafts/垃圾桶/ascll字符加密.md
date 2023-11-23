---
title: ASCLL字符加密
tags: 
  - C
id: '114'
categories:
  - C语言
abbrlink: 59dc3bf9
date: 2021-08-15 12:17:29
---

## 大致要求:

静态:

```c
#include <stdio.h>
int main()
{
    int d, slen = 6;
    char *a = "Weihua";
    printf("%s ", a);
    for (int i = 0; i < slen; i++) {
        if ((i + slen / 2) > 5) {
            d = (int)*(a + i) - (int)*(a + i + slen / 2 - 6);
        } else {
            d = (int)*(a + i) - (int)*(a + i + slen / 2);
        }
        for (; d < 97; d += 26)
            ;
        printf("%c", d);
    }
    return 0;
}
```

新增可动态输入:

```c
#include <stdio.h>
#include <string.h>
int main()
{
    int d, slen;
    char *a, b[20];
    a = b;
    printf("please enter:");
    scanf("%s", a);
    slen = strlen(a);
    printf("%s ", a);
    for (int i = 0; i < slen; i++) {
        if ((i + slen / 2) > 5) //使字符串首位相接
        {
            d = (int)*(a + i) - (int)*(a + i + slen / 2 - 6);
        } else {
            d = (int)*(a + i) - (int)*(a + i + slen / 2);
        }
        for (; d < 97; d += 26)
            ;
        printf("%c", d);
    }
    return 0;
}
```