# rm-code-specification
VANGUARD战队电控组C语言代码规范  
[链接文本](#1文件)  
## 1.文件  
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
## 2.标识符命名  
  
### 2.0 命名总则  
·本规范中使用的标识符的命名方法有 **驼峰命名法** 和 **下划线分隔法** 两种。  
驼峰命名法即为大小写分隔单词，有首字母大写和首字母小写两种形式。例：  
```
int ChassisType;
int chassisType;
```
下划线分隔法使用下划线分隔单词，有纯大写和纯小写两种形式。例：  
```
#define CHASSIS_TYPE
int chassis_type;
```
  
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
·结构体类型的定义统一采用`typedef`定义数据类型的方式，其命名统一为`<name>_t`，其中名称部分采用 **纯小写英文** ，并使用下划线分隔单词。  
结构体的成员，无论其是否为普通变量，一律采用 **纯小写英文** 。例：  
```
typedef struct
{
  struct
  {
    int target_speed_x;
  }move;
} chassis_struct_t;
```
·声明结构体时命名规范与变量命名规范完全一致。例：`chassis_struct_t chassis_1`  

### 2.4 枚举型变量  
·枚举类型的定义与结构体类似，但结尾改为`_e`，且其成员命名统一采用 **纯大写英文** 。例：  
```
enum chassis_type_e
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

## 3.变量使用规范  
·在进行程序编写时，应当尽可能少使用全局变量。如果需要使用全局变量，应当使用`static`关键字进行修饰。除非有极特殊需求，否则不应在一个模块中直接调用另一个模块的全局变量，而是采用函数方式获取该变量的信息。  
例：  
```
static gimbal_t gimbal;

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
  
·在某一模块中，除非有特殊需求，不应使用任何单独的全局变量作为该模块的状态变量，所有状态变量必须作为结构体的成员存在。  
错误示例：`float chassis_wheel_speed[4];`  

·不要在循环内部定义变量。  

·指针和引用变量的`&`、`*`、`.`、`->`后面不应有空格。例：  
```
x = *p;
p = &b;
r.a = 1;
r->b = 2;
```  
  
## 4.换行和缩进规范  
### 4.1 换行  
·除包含长路径的`#include`语句和 **头文件保护** 以外，如果某一语句长度超过了八十个字符，在同一行会影响阅读，则应换行。
·布尔表达式的换行应让逻辑运算符始终在行的末尾，换行后首字符要与上一行对齐。例：  
```
if((...)&&
   (...))
{
  ...
}
```
·两个语句间应换行。  
·所有的`{`均应单独起一行。例：  
```
void Function_Name()
{
  if(1)
  {
  ...
  }
  while(1)
  {
  ...
  }
}
```  
·一般情况下，`if`和`else`应当另起一行。例：  
```
if(a)
{
  ...
}
else if(b)
{
  ...
}
else
{
  ...
}
```
  
### 4.2 缩进  
·代码块内部的语句相比当级代码块外的语句，应有一个制表符长度的缩进。  
·如果某一语句不在代码块中，但其是`if`、`while`、`for`、`case`等语句的下属语句，则也应有一个制表符长度的缩进。  
例：  
```
if(a)
{
  a = c;
  b = d;
  if(b)
    a=b+1;
  switch(a)
  {
    case 1:
      a = 0;
  }
}
```
  
## 5.注释规范  
### 5.1 注释风格  
·注释风格可以选择`//`或`/**/`两种，无硬性要求。   
·所有的注释应添加在被注释对象的上方。  
例：  
```
//这是一个整形变量a
int a;
```
  
### 5.2 文件注释  
·如果该文件是开源文件，则在文件开头加入版权公告.
·如果一个`.h`文件中含有多个概念，则应对该文件进行一个总体的注释，内容为该文件的功能介绍。  
·相反地， 如果一个文件只声明, 或实现, 或测试了一个模块, 并且这个模块已经在它对应的结构体和函数的声明处进行了详细的注释, 那么就没必要再加上文件注释。  
·如果有需要，可以在文件注释中加上作者、版本号和历史更新日期等信息。  
  
### 5.3 结构体注释  
·每个结构体定义时都应该加上对应的注释，对该结构体的功能进行总体的解释，除非该结构体的功能非常显而易见。  
例：  
```
//该结构体为底盘相关状态变量结构体，包含以下部分：
//type ：底盘类型
//info ：底盘基本物理信息
//move ：底盘运动相关状态变量
//motor ：底盘电机相关状态变量
//pid_motor_speed ：底盘电机速度环PID对应结构体
//pid_chassis_angle ：底盘yaw角PID对应结构体
//angle_to_gimbal ：底盘相对于云台的角度
typedef struct
{
	chassis_type_e type;
	struct
	{
    ...
	}info;
	struct
	{
    ...
	}move;
	struct
	{
    ...
	}motor[4];
	PID_t pid_motor_speed[4];
	PID_t pid_chassis_angle;
	float angle_to_gimbal;
}chassis_t;
```

### 5.4 函数注释  
·每个函数声明处前都应当加上注释, 描述函数的功能和用途，同时解释参数和返回值的含义。只有在函数的功能简单而明显时才能省略这些注释。  
例：  
```
/*
 * @brief	底盘功率控制程序
 * @param	chassis	底盘结构体的地址
 * @retval	无
 */
void Chassis_PowerCtrl(chassis_t *chassis)
{
  ...
}
```
### 5.5 变量注释  
·通常变量名本身足以很好说明变量用途. 某些情况下, 也需要额外的注释说明。  
·每个结构体的成员都应该用注释说明用途. 如果有非变量的参数(例如特殊值, 数据成员之间的关系, 生命周期等)不能够用类型与变量名明确表达, 则应当加上注释。然而, 如果变量类型与变量名已经足以描述一个变量, 那么就不再需要加上注释。  
例：  
```
typedef struct
{
  //type: 底盘类型，取值可以是Chassis_Type_e的成员之一
	chassis_type_e type;
  ...
}chassis_t;
```
·所有全局变量也要注释说明含义及用途，例:  
```
//云台状态变量结构体
static gimbal_t gimbal;
```
注意，此处仅为示例，实际使用中应避免将云台状态等变量定义为全局变量。  

### 5.6 TODO注释  
·对那些临时的，短期的解决方案，或是未完成的解决方案，或已经够好但仍不完美的代码使用 `TODO `注释。  
·`TODO`注释采用 **全大写** 的字符串`TODO`，在字符串后的圆括号中写上自己的姓名，同时加上简单的描述。  
例：  
```
void Chassis_Slove(chassis_t *chassis)
{
	//TODO(李四)麦轮底盘解算
	if(chassis->type == Mecanum)
	{
		return;
	}
	//TODO(张三)全向四轮底盘解算
	if(chassis->type == Omni_4)
	{
		return;
	}

}
```

## 6.其他规范  
### 6.1  编码方式  
·在同一工程中，必须统一编码方式。一般统一为GBK编码或UTF-8编码。  
·为防止出现乱码，严禁将某一文件直接保存为不对应的编码格式。  

### 6.2 预处理指令  
·所有预处理指令都不应有缩进，应直接从行首开始。  
例：  
```
#ifndef PI
#define PI 3.1415926535f
#endif
```

### 6.3 变量和数组初始化  
·所有变量和数组的初始化统一使用`=`。例：  
```
int a = 0;
int b[] = {1,1,1,1};
```

### 6.4 函数返回值  
·当函数返回值的表达式较长，可以采用圆括号增加可读性。例：  
```
void Function_Name()
{
  return ((a+b+c)&&(b+c+d));
}
```
·若表达式较长，则不要添加不必要的圆括号。  
错误示例：  
```
void Function_Name()
{
  return (a);
}
```
