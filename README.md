# rm-code-specification
VANGUARD战队电控组C语言代码规范  
## 1. 文件  
### 1.1 头文件  
·头文件的命名应当遵循规范如`<PATH>_<FILE>.h`，模块名与文件名均为 **纯英文小写** ，采用下划线分隔。例：  
```
chassis_task.h
```
·当该模块仅有一个头文件，且完全确定后续不会再添加新的头文件时，可简化为`<PATH>.h`。例：  
```
chassis.h
```
·所有头文件均需使用`#define`进行防止重定义的保护，宏定义的命名规范为`<PATH>_<FLIE>_H`，采用 **全大写**。  

  
·头文件的编写需遵循 **包含、宏定义、类型定义、变量声明、函数声明** 的顺序。  
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
·源文件的命名规范与头文件类似，请参考1.1。  

  
·若源文件为 **STM32CubeMX** 生成的文件，则按照文件内注释进行编写即可。  

·若源文件为自行添加的文件，则按照 **包含、宏定义、变量定义、函数定义** 的顺序进行编写，并尽量不在源文件中使用前置声明。  
例：  
```
#include "xxx.h"
...

#define DEFINE_1 DEFINE_2
...

static int variable_1
static float variable_2
...

void Path_FunctionName()
{
...
}
...
```  
## 2. 标识符命名  
### 2.1 变量  
·变量命名应使用具有具体含义的命名，普通变量命名使用 **纯小写英文** ，采用下划线分隔不同单词。  
例：`int chassis_wheel_speed`  
·在不影响理解、不易混淆的情况下，命名可以采用缩写。  

·当某一变量为单独的变量，而非某一结构体的成员时，命名必须尽可能全面，禁止使用模棱两可的命名。  

  
错误示例：`receinfo`  
错误原因：不同单词未进行分隔，且意义不完全清晰，无法根据变量名判断该变量是从何处接收的信息。   
正确示例：`rece_info_from_nuc`  

·当某一变量为某一结构体的成员时，若结构体命名本身已经有一定含义，则该部分含义可不在变量命名中展现。  
例：`chassis.move.target_speed_x`
  
·当变量为指针类型时，应在变量名末尾加上`_p`，例：`int* pointer_p`  

### 2.2 宏定义  
·宏定义的命名与变量命名类似，仅是在变量命名的基础上，将纯小写英文改为纯大写英文。例：  
```
#define WHEEL_RADIUS 0.15f
```
·当被宏定义的对象为函数时，宏定义的命名必须保留圆括号，禁止将圆括号封装掉。例：  
```
#define FUNCTION() Function()
```

### 2.3 结构体  
·结构体类型的定义统一采用`typedef`定义数据类型的方式，其命名统一为`<name>_t`，其中名称部分采用 **首字母大写** ，并使用下划线分隔单词。  
结构体的成员，无论其是否为普通变量，一律采用 **纯小写英文** 。例：  
```
typedef struct
{
  struct
  {
    int target_speed_x;
  }move;
} Chassis_Struct_t;
```
·声明结构体时命名规范与变量命名规范完全一致。例：`Chassis_Struct_t chassis_1`  

### 2.4 枚举型变量  
·枚举类型的定义与结构体类似，但结尾改为`_e`，且其成员命名统一采用 **纯大写英文** 。例：  
```
enum Chassis_Type_e
{
  MECANMUN = 0,
  OMNI_4 = 1,
  OMNI_3 = 2
};
```
·声明枚举型变量时命名规范与变量命名规范完全一致。例：`Chassis_Type_e`  

### 2.5 函数  
·函数的命名规范遵循`<PATH>_<FUNCTION>`的规范，模块名和函数名部分均采用 **大小写分隔，首字母大写** 的方式，模块名和函数名之间使用下划线分隔。  
例：`bool Chassis_GetMotorData()`  

### 2.6 其他注意事项  
·在定义变量类型时，**严禁** 将指针类型封装起来，并且不做任何标明。  
·如果确有特殊需求，需要将指针类型封装起来，必须在变量类型命名末尾加上大写的`_P`后缀。  
·尽可能少做无意义的定义。  
**严重错误示例**：  
```
typedef struct ReceivePacket _receive_packet;
typedef struct ReceivePacket
{
  ...
}_receivepacket,*_receive_packetinfo;
```
错误原因：  
·命名格式错误  
·定义了多个本质相同但命名不同的数据类型  
·_receivepacket为指针类型，但在命名中未加`_P`后缀  

## 3. 变量使用规范  
·在进行程序编写时，应当尽可能少使用全局变量。如果需要使用全局变量，应当使用`static`关键字进行修饰。除非有极特殊需求，否则不应在一个模块中直接调用另一个模块的全局变量，而是采用函数方式获取该变量的信息。  
例：  
```
static Gimbal_t gimbal;

bool Gimbal_SetTargetAngle(float angle_pitch,float angle_yaw)
{
  gimbal.move.target_angle_pitch = angle_pitch;
  gimbal.move.target_angle_yaw = angle_yaw;
  if(gimbal.move.target_angle_pitch != angle_pitch || gimbal.move.target_angle_yaw != angle_yaw)
  {
    return flase;
  }
  return ture;
}
```
在其他模块中，应当使用函数`Gimbal_SetTargetAngle`对结构体gimbal的成员进行赋值，而非直接调用结构体`gimbal`进行赋值。  

## 4. 换行和缩进规范  
### 4.1 换行  
·除`#include`语句和 **头文件保护** 以外，如果某一语句长度超过了八十个字符，则应换行。
