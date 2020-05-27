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
cp ./reg_files/reg_info.bin ./.reg
make CROSS_COMPILE=aarch64-himix100-linux- u-boot-z.bin
```

