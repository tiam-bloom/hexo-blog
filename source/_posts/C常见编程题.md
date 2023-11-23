---
title: C常见编程题
abbrlink: 4b4998f0
date: 2022-5-19 12:35:16
tags: 
  - C
---

# 2022 06 13

## 1. 累计和

```c
#include<stdio.h>

int main()
{
	int i=1,sum=0;
	while (i<=100)
		sum+=i++;
	printf("%d",sum);	
	return 0;
}
```

## 2. 兔子数列第10项

```c
#include<stdio.h>
int fun(int n){
	if(n<3)
		return 1;
	return fun(n-1)+fun(n-2);
}
int main()
{
	printf("%d",fun(10));	
	return 0;
}
```

## 3. 前10项和

```c
#include<stdio.h>

int main()
{
	int sum= 2,m1=1,m2=1,m,i;
	for(i=2;i<10;i++){
		m=m1+m2;
		sum+=m;
		m1=m2;
		m2=m;
	}
	printf("%d",sum);
	return 0;
}
```

## 4. 公约公倍

```c
#include<stdio.h>
int gongyue(int m,int n){
	if(m%n==0) return n;
	return gongyue(n,m%n);
}
int gongbei(int m,int n){
	return m*n/gongyue(m,n);
}
int main()
{
	int m=36,n=16;
	printf("%d,%d",gongyue(m,n),gongbei(m,n));
	return 0;
}
```

## 5. 水仙花

```c
#include<stdio.h>

int fun(int n){
	return n*n*n;
}

int main()
{
	int i;
	for(i=100;i<1000;i++)
		if(i==fun(i%10)+fun(i/10%10)+fun(i/100))
			printf("%d ",i);
	return 0;
}
```

## 6. 完数

```c
#include<stdio.h>

int fun(int n){
	int i,sum=0;
	for(i=1;i<n;i++)
		if(n%i==0) sum+=i;
	return sum;
}

int main()
{
	int i;
	for(i=1;i<10000;i++)
		if(i==fun(i))
			printf("%d ",i);
	return 0;
}
```

## 7. 阶乘前N项和

```c
#include<stdio.h>

int fun(int n){
	if(n==1)
		return 1;
	return fun(n-1)*n;
}

int main()
{
	int i,sum=0;
	for(i=1;i<=10;i++)
		sum+=fun(i);
	printf("%d",sum);
	return 0;
}
```

## 8. 素数

```c
#include<stdio.h>

int fun(int n){
	int i;
	for(i=2;i<n/2;i++)
		if(n%i==0)
			return 0;
	return 1;
}

int main()
{
	int i;
	for(i=2;i<=100;i++)
		if(fun(i))
			printf("%d ",i);
	return 0;
}
```

## 9. 杨辉三角

```c
#include<stdio.h>

int fun(int m,int n){
	if(m==n || n==0)
		return 1;
	return fun(m-1,n)+fun(m-1,n-1);
}

int main()
{
	int n=10,i,j;
	for(i=0;i<n;i++){
		for(j=0;j<=i;j++)
			printf("%d ",fun(i,j));
		putchar(10);
	}
	return 0;
}
```

## 10. 回文数

```c
#include<stdio.h>
#include<math.h>
//利用math.h中的函数，参数类型易错。
int fun(int n){
	int rev=0,len = (int)log10(n*1.0);
	for(;n;len--,n/=10)
		rev+=(int)pow(10.0,len)*(n%10);
	return rev;
}

int main()
{
	int n;
	scanf("%d",&n);
	printf("%d\n",fun(n));
	printf(n==fun(n)?"Yes":"No");
	return 0;
}
```

```c
#include<stdio.h>
//反转 n
int fun(int n){
	int i=n,m=1,res=0;
	while(i/=10)
		m*=10;
	for(i=n;i;m/=10,i/=10)
		res+=i%10*m;
	return res==n?1:0;
}

int main()
{
	int n;
	scanf("%d",&n);
	printf(fun(n)?"Yes":"No");
	return 0;
}
```

```c
#include<stdio.h>
//从开始与结尾一一对比
int fun(int n){
	int i=n,p=1,q=1;
	while(i/=10)
		p*=10;
	for(;p>q;p/=10,q*=10)
		if(n/p%10!=n/q%10)
			return 0;
	return 1;
}

int main()
{
	int n;
	scanf("%d",&n);
	printf(fun(n)?"Yes":"No");
	return 0;
}
```

## 11. 冒泡排序

```c
void sort(int *a,int n){
	int i,j,temp;
	for(i=0;i<n-1;i++)
		for(j=0;j<n-1-i;j++)
			if(a[j]>a[j+1]){
				temp=a[j];
				a[j]=a[j+1];
				a[j+1]=temp;
			}
}
```

## 12. 选择排序

```c
void sort(int *a,int n){
	int i,j,temp;
	for(i=0;i<n;i++)
		for(j=i+1;j<n;j++)
			if(a[i]>a[j]){
				temp=a[i];
				a[i]=a[j];
				a[j]=temp;
			}
}
```

## 13. 插入排序

```c
void sort(int *a,int n){
	int i,j,temp;
	for(i=1;i<n;i++)
		for(j=i;j>0;j--)
			if(a[j-1]>a[j]){
				temp=a[j];
				a[j]=a[j-1];
				a[j-1]=temp;
			}
}
```

## 14. 反转数组

```c
void reverse(int *a,int n){
	int *p=a+n-1;
	for(;a<p;a++,p--){
		*a=*a^*p;
		*p=*a^*p;
		*a=*a^*p;
	}
}
```

## 15. 反转字符串

```c
#include <stdio.h>

void reverse(char *a){
	char *p=a,temp;
	while(*(p+1)) p++;
	for(;a<p;a++,p--){
		temp=*a;
		*a = *p;
		*p = temp;
	}
}

int main()
{
    char str[20];
	gets(str);
	reverse(str);
	puts(str);
    return 0;
}
```

## 16. 连接字符串

```c
#include <stdio.h>

void strcat_(char *a,char *b){
	while(*a) a++;
	while(*b)
		*a++=*b++;
    *b='\0';
}

int main()
{
    char str[20];
	gets(str);
	strcat_(str,"babico");
	puts(str);
    return 0;
}
```

## 17. 比较字符串

```c
#include <stdio.h>

int strcmp_(char *a,char *b){
	for(;*a==*b;a++,b++)
		if(*a=='\0') return 0;
	return *a-*b;
}


int main()
{
    char str[20];
	gets(str);
	printf("%d",strcmp_(str,"babico"));
    return 0;
}
```

## 18. 复制字符串

```c
#include <stdio.h>

void strcpy_(char *a,char *b){
	while(*a++=*b++);   //b指向0时,已完成赋值.
}

int main()
{
    char str[20];
	strcpy_(str,"babico");
	puts(str);
    return 0;
}
```

## 19. 字符串长度

```c
#include <stdio.h>

int strlen_(char *a){
	int len=0;
	while(*a++) len++;
	return len;
}

int main()
{
    char str[20];
	gets(str);
	printf("%d",strlen_(str));
    return 0;
}
```

## 20. 第几天

```c
#include <stdio.h>

int isleap(int year){
	if(year%400==0||year%4==0&&year%100)
		return 1;
	return 0;
}
int no_day(int year,int month,int day){
	int i,res=0,mon[12]={0,31,28,31,30,31,30,31,31,30,31,30};
	mon[0]=day;
	if(isleap(year)) mon[2]++;
	for(i=0;i<month;i++)
		res+=mon[i];
	return res;
}

int main()
{
    int y,m,d;
	scanf("%d%d%d",&y,&m,&d);
	printf("%d",no_day(y,m,d));
    return 0;
}
```

## 21. 字符串转数字

```c
#include <stdio.h>

int strtonum(char *str){
	char *p=str;
	int m=1,res=0;
	while(p[1]) p++;		
	for(;p>=str;p--,m*=10)
		res += m*(*p-'0');
	return res;
}

int main()
{
	char str[20];
	gets(str);
	printf("%d",strtonum(str));
    return 0;
}
```

## 22. 数字转字符串

 ```c
#include <stdio.h>

void numtostr(char *str,int num){
	int m=1,i=num;
	while(i/=10) m*=10;
	for(;m>0;m/=10,str++)
		*str=num/m%10+48;
	*str='\0';
}

int main(){	
	int num;
	char str[20];
	scanf("%d",&num);
	numtostr(str,num);
	puts(str);
    return 0;
}
 ```

## 23. 折半查找

```c
#include <stdio.h>

int find(int *arr,int n,int num){
	int p=0,q=n-1,mid;
	while(p<=q){
		mid=(p+q)/2;
		if(num>arr[mid])
			p=mid+1;
		else if(num<arr[mid])
			q=mid-1;
		else return mid;
	}
	return -1;
}

int main(){
	int i,arr[10] = {0,1,2,3,4,5,6,7,8,9};
	for(i=-1;i<11;i++)
		printf("%d\n",find(arr,10,i));
    return 0;
}
```

## 

