# u-boot-2016.11

### 文件介绍
略

### 编译流程

1. 选择配置文件
```
make CROSS_COMPILE=aarch64-himix100-linux- hi3559av100_emmc_defconfig 
```

2. 生成u-boot.bin
```
make CROSS_COMPILE=aarch64-himix100-linux-
```

3. 生成最终u-boot-hi3559av100.bin
拷贝reg.bin文件,并执行编译
```
cp ./reg_files/Hi3559AV100-DMEBLITE_8L_T-LPDDR4_2664M_8GB_32bitx2-A73_1608M-soluation3.bin ./.reg
make CROSS_COMPILE=aarch64-himix100-linux- u-boot-z.bin
```

