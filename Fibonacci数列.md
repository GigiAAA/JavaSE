#### 问题描述

Fibonacci数列的递推公式为：Fn=Fn-1+Fn-2，其中F1=F2=1。

当n比较大时，Fn也非常大，现在我们想知道，Fn除以10007的余数是多少。

```c
#define NUMBER 10007
#include<stdio.h>
#include<windows.h>
int numFibonacci(int num) {
	int F[100000] = {0};
	F[1] = F[2] = 1;
	int i = 3;
	for (i = 3; i <= num; i++) {
		F[i] =(F[i - 1] + F[i - 2])%NUMBER;
	}
	return F[i];
}
int main() {
	int num = 0;
	printf("请输入一个正整数:\n");
	scanf("%d\n", &num);
	int result = numFibonacci(num);
	printf("%d\n", result);
	system("pause");
	return 0;
}
```

