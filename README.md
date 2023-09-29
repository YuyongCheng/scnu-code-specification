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

  
头文件的编写需遵循 **包含、宏定义、类型定义、变量声明、函数声明** 的顺序。  
例：  
```
#ifndef CHASSIS_TASK_H
#define CHASSIS_TASK_H

#include "xx.h"

#define DEFINE_1 DEFINE_2

typedef struct
{
...
} xxx_t;

emun xxx_e
{
...
};

extern int variable_1;

extern void Path_FunctionName();

#endif
```
### 1.2 源文件  
源文件的命名规范与头文件类似，请参考1.1。  

  
若源文件为 **STM32CubeMX** 生成的文件，则按照文件内注释进行编写即可。  

若源文件为自行添加的文件，则按照 **包含、宏定义、变量定义、函数定义** 的顺序进行编写，并尽量不在源文件中使用前置声明。  
例：  
```
#include "xxx.h"
...

#define DEFINE_1 DEFINE_2
...

int variable_1
float variable_2
...

void Path_FunctionName()
{
...
}
...
```
