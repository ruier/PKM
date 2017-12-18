# [为何在x86构架中 BIOS 会将 MBR 装载到 0x7c00地址处?                           ](http://blog.csdn.net/bkxiaoc/article/details/50380835)



最近看了操作系统的实现的相关资料，对于这个问题一直想不明白，偶然找到国外博客中的一篇说明？遂简单翻译一下分享出来.

> <http://www.glamenv-septzen.net/en/view/6>

## ‘0x7C00’在x86体系中的秘密

你在x86汇编程序中听说过 `0x7C00` 这个[魔术数字]吗？

`0x7C00`是 BIOS 将 MBR 装载到的位置。

操作系统亦或是引导程序的开发人员必须假设他们的汇编代码被加载到了这个地址，然后从这个地方开始执行。

### **1:**

```
> 我认真的读了 Intel x86(32bit) 的程序手册(简称 Intel手册),但是并没有在里面发现这个[魔术数字]

```

是的，0x7C00 和 x86 CPU 毫无关系。你自然没法从intel手册上面找到他们。 
你会问道，“那是谁决定了它?”

### **2:**

```
> "0x7C00 = 32KiB - 1024B"，起始地址为这个数字到底有何意义?" 

```

无论是谁决定的，他/她为什么要这样决定?

这里就有两个关于 `0x7C00` 的问题了:

> 1. 谁决定的?
> 2. “0x7C00 = 32KiB - 1024B” ,也就是说是 32KB 的最后 1024B为起始地址,这一点有什么特殊含义?

这就要从 `IBM PC 5150` 这台远古时代的 x86 PC 说起了。

## “0x7C00” 第一次出现是在 “IBM PC 5150” 的 BIOS 的 19号中断例程中。

`IBM PC 5150` 是现代 x86 PC 的鼻祖，为了保证向下兼容的原则，”0x7C00”得以保留。

而 19号中断例程 就是 “POST”(Power On Self Test) ，上电自检(包括检查是否存在驱动器)，之后将启动盘的第一分区的第一扇区的 512b 数据拷贝到 0x7c00处。

现在，你应该明白，为何在 intel手册上找不到了吧，因为它属于 BIOS 的规范。

## “0x7C00” 的起源

这个地址的起源和当时的 DOS 系统不无关系，具体的可以看 DOS 的历史。

“0x7c00”是由 IBM PC 5150 BIOS开发团队决定(David Bradley博士)。

如前所述,这[魔术数字]起源于在1981年，之后BIOS供应商和操作系统为了向后兼容，所以并没有改变这个值。

事实上，这个地址既不是由英特尔(8086/8088供应商)决定也不是由微软(OS厂商)决定。

## “0x7C00 = 32KiB - 1024B” 有什么特殊含义? 是为了解决系统和 CPU 内存分布的需求

### BIOS 决定这个地址的理由如下:

1. 32kb是符合(DOS)系统加载的最小空间
2. 8086/8088 0x0 - 0x3ff用于中断向量和BIOS数据区域。
3. 引导扇区是512字节，boot 程序需要的数据/堆栈 大于 512 字节。
4. 因此,0x7c00, 32KB 的最后 1024B 被选中了。

系统加电启动后，可以自由使用 0x7c00起始的, 32KB 的最后 1024B空间。

操作系统加载后,内存布局将是:

> +——————— 0x0 
>   | Interrupts vectors 
>   +——————— 0x400 
>   | BIOS data area 
>   +——————— 0x5?? 
>   | OS load area 
>   +——————— 0x7C00 
>   | Boot sector 
>   +——————— 0x7E00 
>   | Boot data/stack 
>   +——————— 0x7FFF 
>   | (not used) 
>   +——————— (…)



```bash
android@ubuntumysqlserver:~/share/qemu$ hexdump 512.img 
0000000 3ceb 5290 4d41 5344 4e4b 0054 0102 0001
0000010 e002 4000 f00b 0009 0012 0002 0000 0000
0000020 0000 0000 0000 0029 2b60 5281 4d41 4944
0000030 4b53 544e 2020 4146 3154 2032 2020 88fa
0000040 2416 b47c cd08 7213 8311 3fe1 0e89 7c18
0000050 c6fe d232 f286 1689 7c1a 8cfa 8ec8 8ed8
0000060 fcc0 d08e febc fb7b e1be e87d 0114 db33
0000070 c38b d38b 10a0 f77c 1626 8b7c 0e0e 037c
0000080 1c0e 137c 1e16 037c 13c8 89d3 000e 897b
0000090 0216 be7b 7c03 f9bf c77d 3606 207c b900
00000a0 0005 a6f3 0675 06c7 7c36 0040 36a1 f77c
00000b0 1126 8b7c 0b36 037c 48c6 f6f7 c88b a151
00000c0 7b00 168b 7b02 00bb e87e 00d0 0373 a5e9
00000d0 8b00 0b16 bf7c 7e00 ebbe 577d 0bb9 f300
00000e0 5fa6 1c74 3e03 7c36 162b 7c36 ea75 0683
00000f0 7b00 8301 0216 007b e259 bec3 7dea 79eb
0000100 0159 000e 837b 0216 007b c933 1cbb 8300
0000110 363e 407c 0375 c383 8b20 4301 8b43 f711
0000120 0b36 407c 04a3 837b 04eb 018b 4848 0e8a
0000130 7c0d e1f7 0603 7b00 1613 7b02 00bb 8e08
0000140 33c3 50db e852 0054 585a 2a72 f7be e87d
0000150 0030 0eff 7b04 0d74 c083 8301 00d2 c38c
0000160 c383 eb20 a0da 7c24 8024 0675 f2ba 3203
0000170 eec0 2eff 7d7e e6be e87d 0006 feeb 0000
0000180 0800 5053 3e80 7de0 7500 ac0e c00a 0974
0000190 0eb4 07bb cd00 eb10 58f2 c35b 1e89 7b06
00001a0 1e8b 7c18 f3f7 8b42 33da f7d2 1a36 867c
00001b0 b1e0 d206 91e0 cb0a f28a 05bb 5300 1e8b
00001c0 7b06 168a 7c24 01b8 5102 cd52 5a13 7259
00001d0 5b03 c3f8 c033 13cd fe5b 75cf f9e0 f3eb
00001e0 5600 2e31 0036 5221 0064 4221 4f4f 5254
00001f0 4d4f 5320 5359 2b00 5600 4558 5458 aa55
0000200
```

```c
/* use this macro to put the relative jump to offset 0x3E
 * into the boot sector and copy the vxWorks bootstrap code
 * into the boot sector code area
 */
#define BUILD_BOOTSEC(x) \
    { /* modify first 3 bytes and copy bootstrap to offset 0x3E */ \
    x[0] = 0xeb;   /* x86 relative jump inst */	 \
    x[1] = 0x3c;   /* rel address for jmp */	 \
    x[2] = 0x90;   /* x86 nop */		 \
    bcopy ((const char *)bootStrap, (char *)&x[0x3e], sizeof (bootStrap));  \
    }

/* global */
```

vxworks  起始地址上的机器码是 eb 3c 90，对应的就是跳转到 3e 的位置：

https://www.cnblogs.com/sunt/archive/2010/11/25/1887657.html

如果我们在第一行程序后加上Mov bx,0000,你会发器机器码没变，还是EB03,为什么呢?jmp 
0008对应的偏移就是0003大家可以回忆一下cpu中指令的执行流程，就会发现当执行完EB03后，ip=ip+2=0005,大家注意看EB03后面有个03，表示再向后三个单位，这样就到了0008这个偏移处了。所以我们说包含 的是转移的位移。

后面接着是加载 boot sys 的代码，这段 512 字节的代码可以直接做成镜像，启动，当然显示没有找到，在 virtual box 中可以打印出来。