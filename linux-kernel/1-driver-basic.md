# Linux 驱动基础

## Linux 源码顶层目录

- **arch** 架构相关目录，里面存放了许多 CPU 的架构
- **block** 存放块设备相关代码
- **crypto** 存放加密算法目录
- **Documentation** 存放官方 Linux 内核文档
- **drivers** 驱动目录，存放了 Linux 系统支持的硬件设备驱动源码
- **firmware** 存放固件目录
- **fs** 存放支持的文件系统的代码目录
- **include** 存放公共的头文件目录
- **init** 存放 Linux 内核启动初始化的代码
- **ipc** 存放进程间通信代码
- **kernel** 存放内核本身的代码文件夹
- **lib** 存放库函数的文件夹
- **mm** 存放内存管理的目录
- **net** 存放网络相关代码
- **scripts** 存放脚本的文件夹
- **security** 存放安全相关代码
- **sound** 存放音频相关代码
- **tools** 存放 Linux 用到的工具文件夹
- **usr** 和 Linux 内核的启动有关的代码
- **virt** 内核虚拟机相关代码

## 内核模块命令

- `insmod` 载入 Linux 内核模块
- `modprobe` 加载 Linux 内核模块，同时这个模块所依赖的模块也同时被加载
- `rmmod` 移除已载入 Linux 的内核模块
- `lsmod` 列出已经载入 Linux 的内核模块，也可以通过 `cat /proc/modules` 查看模块是否加载成功
- `modinfo` 查看内核模块信息

## 函数、宏

### `<linux/module.h>`

*内核模块基础操作。*

- `module_init()` Linux 内核加载驱动
- `module_exit()` Linux 内核卸载驱动
- `MODULE_LICENSE()` 声明驱动的 License
- `MODULE_AUTHOR()` 声明驱动的开发者
- `MODULE_VERSION()` 声明驱动的版本信息

*导出的模块可以被其他模块使用。*

- `EXPORT_SYMBOL()` 导出符号到内核符号表中
- `EXPORT_SYMBOL_GPL()` 导出符号到内核符号表中，只适用于包含 GPL 许可的模块

### `<include/linux/moduleparam.h>`

*为内核模块传递参数。*

- `module_param()` 为驱动传递参数，传递基本类型
- `module_param_array()` 为驱动传递参数，传递数组类型
- `module_param_string()` 为驱动传递参数，传递字符串类型
- `MODULE_PARM_DESC()` 描述模块参数的信息

## 内核错误处理

内核基本的错误码保存在 `errno-base.h` 文件中。

内核中保留了一段地址用来记录错误码。内核中的函数如果返回一个指针，可能会有三种情况：合法指针、NULL指针和非法指针。使用 `IS_ERR()` 检查函数返回值，如果地址落在内核记录错误码的保留地址，表示函数执行失败，`IS_ERR()` 会返回 1。使用 `PTR_ERR()` 可以从这个地址获取到对应的错误码。`IS_ERR()` 和 `PTR_ERR()` 定义在 `errno.h` 中。