### 寄存器配置文件夹


#### 文件介绍

```
--reg_files
    |- Hi3559AV100-DMEBLITE_8L_T-LPDDR4_2664M_8GB_32bitx2-A73_1608M-soluation3.bin     /* 已适配的reg_info.bin文件 */
    |- mkboot.sh        /* 重新压缩制作uboot文件工具 */
    |- README.md        /* readme说明文件 */
```

#### .bin文件
此类文件是指导bootrom进行初始化寄存器的配置文件。

根据开发板的不同需要定制修改DDR、CFGIO、PERI等寄存器。

海思SDK中提供了标准的excel配置文件,并内嵌了**宏函数**进行直接生成配置文件reg\_info.bin

SDK相关路径路径：
    [Hi3559AV100R001C02SPC031\02.only for reference\uboot]()

> 配置文件最大为10K，由脚本工具直接将其填充在uboot生成文件的64字节起始位置:

- 文件格式分析：

'''
root@alex# od -A x -t x1z -v ./reg\_info.bin
000000 34 00 01 12 01 00 00 00 00 00 00 00 05 b8 00 00  >4...............<
000010 34 00 01 12 00 00 00 00 00 00 00 00 05 b8 00 00  >4...............<
000020 10 01 01 12 01 00 00 00 00 00 00 00 05 88 00 00  >................<
......
......
'''

每16个字节为一条寄存器信息,具体的格式如下：
Byte
0~3：  寄存器地址
4~7：  寄存器值
8~11： 延时时间
12~15: 寄存器属性（读写方向、位数）


#### 最终文件生成

最终烧录的文件 **u-boot-hi3559av100.bin** 由两部分组成：u-boot.bin 和 .reg 文件


```
+----------------+     +-------------+
|   u-boot.bin   |  +  |    .reg     |
+----------------+     +-------------+
                   ||
                   ||
       +--------------------------+
       | |   .reg  |  u-boot.bin  |
       +--------------------------+
         ^         ^
         64Byte   10K+64Byte
```

> uboot编译中使用u-boot-z.bin就是执行以上压缩操作,需要将reg\_info.bin文件重命名.reg放到源码根目录下
> 参考Makefile实现：../arch/arm/cpu/armv8/hi3559av100/hw\_compressed/Makefile 

#### 修改工具mkboot.sh介绍

在海思SDK中提供了shell脚本程序mkboot.sh，将最终文件 u-boot-hi3559av100.bin 和 修改后的 reg\_info.bin 重新合并

```
root@alex-virtual-machine:~/git/u-boot-2016.11/reg_files# ./mkboot.sh 

usage:
     mkboot.sh <config file> <output file>
          e.g.
               mkboot.sh reg_info.bin u-boot-ok.bin
```


### Changelog

#### 2020-05-27
- 在reg\_cfg.bin中添加gpio相关控制, 设置GPIO16\_0~2三个输出高电平，用于为usb口提供5V电压输出.

#### 2020-05-23
- 适配修改LPDDR参数，采用《Hi3559A╱C V100 LPDDR4 参数配置方法.pdf》中提及的方案三修改（由LPDDR芯片决定）

