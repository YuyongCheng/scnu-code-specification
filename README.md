# rm-code-specification
VANGUARD战队电控组C语言代码规范  
## 1.文件  
### 1.1 头文件  
头文件的命名应当遵循规范如`<PATH>_<FILE>.h`，模块名与文件名均为 **首字母大写** ，采用下划线分隔。例：  
```
Chassis_Task.h
```
当该模块仅有一个头文件，且完全确定后续不会再添加新的头文件时，可简化为`<PATH>.h`。例：  
```
Chassis.h
```
所有头文件均需使用`#define`进行防止重定义的保护，宏定义的命名规范为`<PATH>_<FLIE>_H`，采用 **全大写**。  
进行`#define`保护需遵循一定规范，例：  
```
#ifndef CHASSIS_TASK_H
#define CHASSIS_TASK_H

...

#endif
```
