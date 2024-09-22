# Linux 字符设备基础

## 设备号

**设备号**的数据类型为 `dev_t`，定义在 `include/linux/types.h` 中。

设备号是一个 32 位的数据，其中高 12 位为主设备号，低 20 位为次设备号。

**主设备号**表示设备类型，**次设备号**用于区分同种类型的不同设备。

### 查看设备号

- `cat /proc/devices` 可以查看所有已申请的主设备号
- `ls -l <dev>` 可以列出设备文件的主设备号和次设备号

### 操作设备号

`<include/linux/kdev_t.h>`

- `MAJOR()` 从 `dev_t` 中获取主设备号
- `MINOR()` 从 `dev_t` 中获取从设备号
- `MKDEV()` 用于将主设备号和从设备号组合成设备号

## 设备节点

### 操作设备节点

`mknod` 手动创建设备节点

## 函数、宏、数据类型

### `<include/linux/fs.h>`

*注册字符设备号。*

- `register_chrdev_region()` 静态注册字符设备号
- `alloc_chrdev_region()` 动态分配字符设备号
- `unregister_chrdev_region()` 释放字符设备号

### `<include/linux/cdev.h>`

*操作字符设备。*

- `struct cdev` 描述一个字符设备
- `cdev_init()` 初始化 `cdev` 结构体成员
- `cdev_add()` 向系统添加一个字符设备
- `cdev_del()` 从系统中删除一个字符设备

### `<include/linux/fs.h>`

*文件操作。*

- `struct file_operations` 描述操作文件的结构体

### `<include/linux/device.h>`

*操作设备节点。*

- `class_create()` 创建类，并在 `/sys/class` 路径下创建文件
- `device_create()` 在类下创建一个设备，并在 `/dev` 路径下创建设备节点
- `device_destroy()` 删除设备
- `class_destroy()` 删除类

### `<include/linux/asm-arm/uacces.h>`

*用户空间与内核空间交互数据。*

- `copy_from_user()` 把用户空间的数据复制到内核空间
- `copy_to_user()` 把内核空间的数据复制到用户空间

### `<include/linux/miscdevice.h>`

*杂项设备。*

- `struct miscdevice` 描述杂项设备
- `MISC_DYNAMIC_MINOR` 自动分配次设备号
- `misc_register()` 注册杂项设备
- `misc_deregister()` 卸载杂项设备
