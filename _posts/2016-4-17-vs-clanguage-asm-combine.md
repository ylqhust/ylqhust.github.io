---
layout: post
title: vs2015 C语言中调用汇编子程序
date: 2016-4-17
categories: blog
tags: [汇编,C语言,VS2015]
description: 如何在C语言中调用汇编子程序
---
# VS2015，C语言中调用汇编子程序  
C语言中调用汇编子程序，先上源代码  

	//main.c源文件
	#include <stdio.h>

	#pragma inline

	extern void sum(char a,char b);//这里是引用的汇编子程序中的求和程序
	int main(int argc, const char * argv[])
	{
		int result;
		sum(10, 11);
		__asm mov result,eax
		printf("%d\n", result);
		return 0;
	}
	
	//sums.asm汇编源文件
	public _sum;函数声明为外部可引用的
	_text segment word 'code'
	assume cs:_text
	_sum proc far;函数名前面必须加上'_'，否则c无法识别
	push bp
	mov bp,sp
	push si
	mov si,bp
	mov dx,ss:[bp+8]
	mov ax,ss:[bp+12]
	add ax,dx
	pop si
	pop bp
	retn
	_sum endp
	_text ends
	end
	
接下来说说如何连接，调试  
### 连接  
	1.将
	C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin
	地址加入到环境变量中，不同的VS版本可能不同，但主要目的就是为了方便使用ml.exe工具
	
	打开cmd命令窗口，输入
	ml /c /coff sums.asm
	完成后，会生成一个sums.obj文件，这个是需要给c文件用的  
	
	2.在VS2015的解决方案管理器中，右键项目名-》添加-》现有项
	将sums.obj添加到项目中，添加完成后，项目目录下会有一个sums.obj文件
	
	3.点击本地windows调试器，就可以运行了

### 调试  
	在调试程序的时候，点击顶部工具栏中的 
	调试->窗口->反汇编(Ctrl+Alt+D 如果程序没有运行，那么将不会有反汇编这一选项)
	可以一步步的查看反汇编代码，这是十分重要的，因为sums.obj文件中的汇编代码可能和你源文件中的汇编代码完全不同，所以必须通过这一步加以确认，修改，否则程序将执行出错
	比如说我上面的sums.asm文件的反汇编代码是这样的  
	push ebp
	mov ebp,esp
	push esi
	mov edx,dword ptr [esi+8]
	mov eax,dword ptr [esi+0Ch]
	add eax,edx
	pop esi
	pop ebp
	ret

![](https://raw.githubusercontent.com/ylqhust/ylqhust.github.io/master/_posts/img/2016-4-18-17-29.png)

	可以看到，我原本使用的是bp，这里变成了esi，这也是为什么我必须将si压栈，将bp赋值给si的原因
	
	调试->窗口->寄存器(Ctrl+Alt+G 如果程序没有运行，那么将不会有寄存器这一选项)
	这里可以看到每一个寄存器中的值
	
	