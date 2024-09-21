---
title: 嵌入式程序设计
date: 2024-06-12 15:40:35
toc: true
tags: 
    - Linux
    - Embedded System
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/covers/xdu/wp13507399-linux-4k-pc-wallpapers.1758k5k5us.webp
categories: collaboration
excerpt: 计科院嵌入式程序设计课程学习笔记
---

## 前言
{% message color:info %}
[课程实验、复习、期末试题资源详见阿里云盘链接](https://www.alipan.com/s/cwbkw17j95p)
{% endmessage %}

{% message color:info %}
博主是21级计科院的，在此澄清一下部分学长留存的实验经验：
+ 实验比较基础，并不像之前所说非常棘手；
+ 可以提前准备，但也没太大必要，认真做下午基本就能完全结束；
+ 实验三代码需要自己编写，但结构清晰，较为容易；
{% endmessage %}

## 思维导图
<iframe src="/pdfjs/web/viewer.html?file=/pdf/collaboration/EmbeddedProgramming.pdf" style='width:100%;height:800px'></iframe>

## 用 Qt 编写嵌入式 GUI 程序——加减乘除四则运算器设计
### 头文件
```c calculator.h
#ifndef CALCULATOR_H
#define CALCULATOR_H //对 calculator.h 头文件的声明
#include <QWidget> //包含主窗体类
#include <QPushButton> //包含按键类
#include <QVBoxLayout> //包含垂直布局器类
#include <QHBoxLayout> //包含水平布局器类
#include <QLineEdit> //包含显示框类
#include <QStack>
class Calculator : public QWidget //计算器继承自主窗体类
{
 Q_OBJECT //必须加上这句，如果要调用信号，槽函数的操作的话
public:
 Calculator(); //计算器类的构造函数
 ~Calculator(); //计算器类的析构函数
public slots: //定义各个按键按下后对应操作处理的槽函数
 void zeroButtonPress();
 void oneButtonPress();
 void twoButtonPress();
 void threeButtonPress();
 void fourButtonPress();
 void fiveButtonPress();
 void sixButtonPress();
 void sevenButtonPress();
 void eightButtonPress();
 void nineButtonPress();
 void addButtonPress();
 void subButtonPress();
 void mulButtonPress();
 void divButtonPress();
 void clearButtonPress();
 void equButtonPress();
private:
 QLineEdit *operateEdit;//声明显示框
 QPushButton *zeroButton;//声明数字按键¹
 QPushButton *oneButton;
 QPushButton *twoButton;
 QPushButton *threeButton;
 QPushButton *fourButton;
 QPushButton *fiveButton;
 QPushButton *sixButton;
 QPushButton *sevenButton;
 QPushButton *eightButton;
 QPushButton *nineButton;
 QPushButton *clearButton;//声明运算符按键
 QPushButton *addButton;
 QPushButton *subButton;
 QPushButton *divButton;
 QPushButton *mulButton;
 QPushButton *equButton;
 QHBoxLayout *firstLayout;//声明水平布局器，该布局器主要对 16 个按键进行布局
 QHBoxLayout *secondLayout;
 QHBoxLayout *thirdLayout;
 QHBoxLayout *fourthLayout;
 QVBoxLayout *mainLayout;//声明垂直布局器，该布局器主要对主窗体上面的空间进行排布
 QString expression;
 int precedence(QChar op);
 float calculator(float a,float b,QChar op);
};
#endif // CALCULATOR_H
```
### 源文件
```c++ calculator.cpp
#include"calculator.h"
Calculator::Calculator()
{
 operateEdit = new QLineEdit(this);//初始化显示框
 operateEdit->setReadOnly(true); //设置显示框为只读
 operateEdit->setText(tr("0"));//初始化显示框显示数据为0
 zeroButton = new QPushButton;//初始化按键
 zeroButton->setText(tr("0"));//设置按键上显示的标签，以下对按键相关的操作类似
 oneButton = new QPushButton;
 oneButton->setText(tr("1"));
 twoButton = new QPushButton;
 twoButton->setText(tr("2"));
 threeButton = new QPushButton;
 threeButton->setText(tr("3"));
 fourButton = new QPushButton;
 fourButton->setText(tr("4"));
 fiveButton = new QPushButton;
 fiveButton->setText(tr("5"));
 sixButton = new QPushButton;
 sixButton->setText(tr("6"));
 sevenButton = new QPushButton;
 sevenButton->setText(tr("7"));
 eightButton = new QPushButton;
 eightButton->setText(tr("8"));
 nineButton = new QPushButton;
 nineButton->setText(tr("9"));
 clearButton = new QPushButton;
 clearButton->setText(tr("Clear"));
 addButton = new QPushButton;
 addButton->setText(tr("+"));
 subButton = new QPushButton;
 subButton->setText(tr("-"));
 mulButton = new QPushButton;
 mulButton->setText(tr("*"));
 divButton = new QPushButton;
 divButton->setText(tr("/"));
 equButton = new QPushButton;
 equButton->setText(tr("="));
 firstLayout = new QHBoxLayout;//初始化水平布局器 firstLayout
 firstLayout->addWidget(zeroButton); //把按键 zeroButton 添加到 firstLayout
 firstLayout->addWidget(oneButton); //把按键 oneButton 添加到 firstLayout
 firstLayout->addWidget(twoButton); //把按键 twoButton 添加到 firstLayout
 firstLayout->addWidget(addButton); //把按键 threeButton 添加到 firstLayout，以下对水平布局器的操作类似
 secondLayout = new QHBoxLayout;
 secondLayout->addWidget(threeButton);
 secondLayout->addWidget(fourButton);
 secondLayout->addWidget(fiveButton);
 secondLayout->addWidget(subButton);
 thirdLayout = new QHBoxLayout;
 thirdLayout->addWidget(sixButton);
 thirdLayout->addWidget(sevenButton);
 thirdLayout->addWidget(eightButton);
 thirdLayout->addWidget(mulButton);
 fourthLayout = new QHBoxLayout;
 fourthLayout->addWidget(nineButton);
 fourthLayout->addWidget(clearButton);
 fourthLayout->addWidget(equButton);
 fourthLayout->addWidget(divButton);
 mainLayout = new QVBoxLayout(this);//初始化垂直布局器 mainLayout
 mainLayout->addWidget(operateEdit); //把显示数据框 operateEdit 加到 mainLayout
 mainLayout->addLayout(firstLayout); //把水平布局器 firstLayout 添加到 mainLayout
 mainLayout->addLayout(secondLayout); //把水平布局器 secondLayout 添加到mainLayout
 mainLayout->addLayout(thirdLayout); //把水平布局器 thirdLayout 添加到 mainLayout
 mainLayout->addLayout(fourthLayout); //把水平布局器 fourthLayout 添加到 mainLayout
connect(zeroButton,SIGNAL(clicked()),this,SLOT(zeroButtonPress()));
//把按键 zeroButton 的按下事件同 zeroButtonPress()绑定到一起，以下操作类似
 connect(oneButton,SIGNAL(clicked()),this,SLOT(oneButtonPress()));
 connect(twoButton,SIGNAL(clicked()),this,SLOT(twoButtonPress()));
 connect(threeButton,SIGNAL(clicked()),this,SLOT(threeButtonPress()));
 connect(fourButton,SIGNAL(clicked()),this,SLOT(fourButtonPress()));
 connect(fiveButton,SIGNAL(clicked()),this,SLOT(fiveButtonPress()));
 connect(sixButton,SIGNAL(clicked()),this,SLOT(sixButtonPress()));
 connect(sevenButton,SIGNAL(clicked()),this,SLOT(sevenButtonPress()));
 connect(eightButton,SIGNAL(clicked()),this,SLOT(eightButtonPress()));
 connect(nineButton,SIGNAL(clicked()),this,SLOT(nineButtonPress()));
 connect(addButton,SIGNAL(clicked()),this,SLOT(addButtonPress()));
 connect(subButton,SIGNAL(clicked()),this,SLOT(subButtonPress()));
 connect(mulButton,SIGNAL(clicked()),this,SLOT(mulButtonPress()));
 connect(divButton,SIGNAL(clicked()),this,SLOT(divButtonPress()));
 connect(equButton,SIGNAL(clicked()),this,SLOT(equButtonPress()));
 connect(clearButton,SIGNAL(clicked()),this,SLOT(clearButtonPress()));
 this->setWindowTitle(tr("Calculator"));//设置窗体标题为 Calculator
 expression="";
}

Calculator::~Calculator()
{
    if (operateEdit != NULL)
    {
        delete operateEdit;
        operateEdit = NULL;
    }

    if (zeroButton != NULL)
    {
        delete zeroButton;
        zeroButton = NULL;
    }

    if (oneButton != NULL)
    {
        delete oneButton;
        oneButton = NULL;
    }

    if (twoButton != NULL)
    {
        delete twoButton;
        twoButton = NULL;
    }

    if (threeButton != NULL)
    {
        delete threeButton;
        threeButton = NULL;
    }

    if (fourButton != NULL)
    {
        delete fourButton;
        fourButton = NULL;
    }

    if (fiveButton != NULL)
    {
        delete fiveButton;
        fiveButton = NULL;
    }

    if (sixButton != NULL)
    {
        delete sixButton;
        sixButton = NULL;
    }

    if (sevenButton != NULL)
    {
        delete sevenButton;
        sevenButton = NULL;
    }

    if (eightButton != NULL)
    {
        delete eightButton;
        eightButton = NULL;
    }

    if (nineButton != NULL)
    {
        delete nineButton;
        nineButton = NULL;
    }

    if (clearButton != NULL)
    {
        delete clearButton;
        clearButton = NULL;
    }

    if (addButton != NULL)
    {
        delete addButton;
        addButton = NULL;
    }

    if (subButton != NULL)
    {
        delete subButton;
        subButton = NULL;
    }

    if (mulButton != NULL)
    {
        delete mulButton;
        mulButton = NULL;
    }

    if (divButton != NULL)
    {
        delete divButton;
        divButton = NULL;
    }

    if (equButton != NULL)
    {
        delete equButton;
        equButton = NULL;
    }

    if (firstLayout != NULL)
    {
        delete firstLayout;
        firstLayout = NULL;
    }

    if (secondLayout != NULL)
    {
        delete secondLayout;
        secondLayout = NULL;
    }

    if (thirdLayout != NULL)
    {
        delete thirdLayout;
        thirdLayout = NULL;
    }

    if (fourthLayout != NULL)
    {
        delete fourthLayout;
        fourthLayout = NULL;
    }

    if (mainLayout != NULL)
    {
        delete mainLayout;
        mainLayout = NULL;
    }
}



void Calculator::zeroButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression += "0";
    } else {
        expression.append("0");
    }
    operateEdit->setText(expression);
}

void Calculator::oneButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "1";
    } else {
        expression.append("1");
    }
    operateEdit->setText(expression);
}

void Calculator::twoButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "2";
    } else {
        expression.append("2");
    }
    operateEdit->setText(expression);
}

void Calculator::threeButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "3";
    } else {
        expression.append("3");
    }
    operateEdit->setText(expression);
}

void Calculator::fourButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "4";
    } else {
        expression.append("4");
    }
    operateEdit->setText(expression);
}

void Calculator::fiveButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "5";
    } else {
        expression.append("5");
    }
    operateEdit->setText(expression);
}

void Calculator::sixButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "6";
    } else {
        expression.append("6");
    }
    operateEdit->setText(expression);
}

void Calculator::sevenButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "7";
    } else {
        expression.append("7");
    }
    operateEdit->setText(expression);
}

void Calculator::eightButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "8";
    } else {
        expression.append("8");
    }
    operateEdit->setText(expression);
}

void Calculator::nineButtonPress()
{
    if(expression.isEmpty() || expression.endsWith('0')) {
        expression = "9";
    } else {
        expression.append("9");
    }
    operateEdit->setText(expression);
}

void Calculator::addButtonPress()
{
    if(!expression.isEmpty() && !expression.endsWith('+') && !expression.endsWith('-') && !expression.endsWith('*') && !expression.endsWith('/')) {
        expression.append("+");
    }
    operateEdit->setText(expression);
}

void Calculator::subButtonPress()
{
    if(!expression.isEmpty() && !expression.endsWith('+') && !expression.endsWith('-') && !expression.endsWith('*') && !expression.endsWith('/')) {
        expression.append("-");
    }
    operateEdit->setText(expression);
}

void Calculator::mulButtonPress()
{
    if(!expression.isEmpty() && !expression.endsWith('+') && !expression.endsWith('-') && !expression.endsWith('*') && !expression.endsWith('/')) {
        expression.append("*");
    }
    operateEdit->setText(expression);
}

void Calculator::divButtonPress()
{
    if(!expression.isEmpty() && !expression.endsWith('+') && !expression.endsWith('-') && !expression.endsWith('*') && !expression.endsWith('/')) {
        expression.append("/");
    }
    operateEdit->setText(expression);
}

void Calculator::clearButtonPress()
{
    expression.clear();
    operateEdit->setText("0");
}
//等号操作响应函数
void Calculator::equButtonPress()
{
    if(expression.isEmpty()) {
        return;
    }

    QStack<float> numbers;
    QStack<QChar> operators;
    QString number;
    for(int i = 0; i < expression.size(); ++i) {
        QChar c = expression.at(i);
        if(c.isDigit() || c == '.') {
            number.append(c);
        } else {
            if(!number.isEmpty()) {
                numbers.push(number.toFloat());
                number.clear();
            }
            while(!operators.isEmpty() && precedence(operators.top()) >= precedence(c)) {
                float second = numbers.pop();
                float first = numbers.pop();
                QChar op = operators.pop();
                numbers.push(calculator(first, second, op));
            }
            operators.push(c);
        }
    }
    if(!number.isEmpty()) {
        numbers.push(number.toFloat());
    }
    while(!operators.isEmpty()) {
        float second = numbers.pop();
        float first = numbers.pop();
        QChar op = operators.pop();
        numbers.push(calculator(first, second, op));
    }
    float result = numbers.pop();
    expression = QString::number(result, 'f', 7);
    operateEdit->setText(expression);
}

int Calculator::precedence(QChar op)
{
    if(op == '+' || op == '-') {
        return 1;
    } else if(op == '*' || op == '/') {
        return 2;
    }
    return 0;
}

float Calculator::calculator(float a, float b, QChar op)
{
    switch(op.toLatin1()) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
    }
    return 0;
}
```
### 主函数
```c++ main.cpp
#include <QApplication>//包含应用程序类
#include "calculator.h"//包含计算器类
int main(int argc, char *argv[])//main 函数的标准写法
{
 QApplication app(argc, argv); //创建一个 QApplication 对象，管理应用程序的资源
 Calculator mainwindow; //产生一个计算器对象
 mainwindow.showMaximized();//显示计算器窗体(默认以最大化的形式显示)
 return app.exec();//让程序进入消息循环，等待可能的菜单、工具条、鼠标等的输入，
}
```
## 编写嵌入式 Linux 设备驱动程序——LED驱动程序
### Notes
+ `insmod led_driver.ko`   加载 `LED` 驱动模块
+ `lsmod`  查看目标机中已经加载的所有模块
+ `cat /proc/devices`  查看目标机中已经加载的所有设备，包括字符设备、块设备
+ `rmmod led_driver.ko`  卸载 `LED` 驱动模块
+ `mknod /dev/XXX c #### x`  创建 `LED` 设备文件节点
    + `XXX` 表示设备名，Linux 根据设备名创建设备目录；本实验中表示 `led_light2` 即指 `LED` 设备的第二个驱动程序，默认驱动程序为 `led_light`;
    + `c` 表示字符设备
    + `####` 表示主设备号
    + `x` 表示次设备号
+ `ls /dev`  查看创建的所有设备
+ `rmmod led_driver.ko` 卸载 `LED` 驱动模块

### Codes Modification
实验手册中给定的模块删除代码为：
```c
/*模块的退出*/
static void __exit s5pv210_led_exit(void)
{
/* -------------------------------- */
device_destroy(led_dev_class,DEVICE_NODE);
/* ---------------------------------- */
class_destroy(led_dev_class);
cdev_del(cdev_p);
unregister_chrdev_region(num_dev,1);
}
```
根据设备文件节点是无法移除设备的，Linux 通过分配的设备号（主设备号、次设备号）来注册设备的，因此需要删除设备对应的设备号 `num_dev`，修改后的代码为：
```c
/*模块的退出*/
static void __exit s5pv210_led_exit(void)
{
/* -------------------------------- */
device_destroy(led_dev_class,num_dev);
/* ---------------------------------- */
class_destroy(led_dev_class);
cdev_del(cdev_p);
unregister_chrdev_region(num_dev,1);
}
```

### 模块驱动程序
```c led_driver.c
#include<linux/init.h>
#include<linux/module.h>
#include<linux/sched.h>
#include<linux/kernel.h>
#include <asm/uaccess.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>

#include <linux/cdev.h>
#include <linux/fs.h> 
#include <linux/device.h>
/*定义设备目录*/
#define DEVICE_LIST "led_test2"
/*定义设备文件节点*/
#define DEVICE_NODE "led_light2"
#define LED1 0x01
#define LED2 0x02
#define LED3 0x04
#define LED4 0x08
/*定义申请设备号(主设备号+次设备号)的变量*/
static dev_t num_dev; 
/*字符设备的变量定义*/
static struct cdev *cdev_p;

/*定义一个 class 类*/
static struct class *led_dev_class;
/*定义一个全局变量，表示 LED 灯的状态*/
static unsigned char led_status = 0;
/*设置 LED 灯的状态*/
static void set_led_status(unsigned char status)
{
/*表示 LED 灯的状态是否发生变化*/
unsigned char led_status_changed;
led_status_changed= led_status^(status & 0xF);
/*数据变化检测*/
led_status=(status & 0xF);
/*如果 4 个 LED 灯的状态发生了变化*/

if(led_status_changed!=0x00)
{
/*判断是否改变 LED1 灯的状态*/
if(led_status_changed&LED1)
{

if(led_status&LED1)
gpio_direction_output(S5PV210_GPH0(0),0);
else
gpio_direction_output(S5PV210_GPH0(0),1);
}
/*判断是否改变 LED2 灯的状态*/
if(led_status_changed&LED2)
{
if(led_status&LED2)
gpio_direction_output(S5PV210_GPH0(1),0);
else
gpio_direction_output(S5PV210_GPH0(1),1);
}

/*判断是否改变 LED3 灯的状态*/
if(led_status_changed&LED3)
{
if(led_status&LED3)
gpio_direction_output(S5PV210_GPH0(2),0);
else
gpio_direction_output(S5PV210_GPH0(2),1);
}
/*判断是否改变 LED4 灯的状态*/
if(led_status_changed&LED4)
{
if(led_status&LED4)
gpio_direction_output(S5PV210_GPH0(3),0);
else
gpio_direction_output(S5PV210_GPH0(3),1);
} } }

/*读取 LED 灯的状态*/
static ssize_t s5pv210_led_read(struct file * file,char * buf,size_t count,loff_t * f_ops)
{
 /*从用户空间读取数据,获取 LED 灯的状态*/
 copy_to_user(buf, (char *)&led_status, sizeof(unsigned char));
 return sizeof(unsigned char); 
}

/*定义实现 LED 灯的写操作*/
static ssize_t s5pv210_led_write (struct file * file,const char * buf, size_t count,loff_t * f_ops)
{
unsigned char status;
if(count==1)
{
/*向用户空间写数据,如果写失败，则返回错误*/
if(copy_from_user(&status, buf,sizeof(unsigned char)))
return -EFAULT;
set_led_status(status);
return sizeof(unsigned char);
 }else
 return -EFAULT;
}

/*打开 LED 设备*/
static ssize_t s5pv210_led_open(struct inode * inode,struct file * file)
{ /*增加管理此设备的 owner 模块的使用计数*/
try_module_get(THIS_MODULE);
return 0;
}
/*释放 LED 设备*/
static ssize_t s5pv210_led_release(struct inode * inode, struct file * file)
{
/*减少管理此设备的 owner 模块的使用计数*/
module_put(THIS_MODULE);
return 0;
}

/*定义具体的文件操作*/
static const struct file_operations s5pv210_led_ctrl_ops={
 .owner = THIS_MODULE,
 .open = s5pv210_led_open,
 .read = s5pv210_led_read,
 .write = s5pv210_led_write,
 .release = s5pv210_led_release,
};
/*LED 灯的初始化和 LED 设备驱动的加载*/
static int s5pv210_led_ctrl_init(void)
{
int err;

struct device* temp=NULL;
unsigned int gpio;
/*GPIO 口的初始化 LED1,LED2,LED3,LED4，设置为输出*/
for(gpio=S5PV210_GPH0(0);gpio<S5PV210_GPH0(4);gpio++)
{
s3c_gpio_cfgpin(gpio, S3C_GPIO_SFN(1));
}
/*动态注册 led_test 设备,num_dev 为动态分配出来的设备号(主设备号+次设备号)*/
err=alloc_chrdev_region(&num_dev,0,1,DEVICE_LIST);
if (err < 0) {
printk(KERN_ERR "LED: unable to get device name %d/n", err);
return err;
}
/*动态分配 cdev 内存空间*/
cdev_p = cdev_alloc();
cdev_p->ops = &s5pv210_led_ctrl_ops;

/*加载设备驱动*/
err=cdev_add(cdev_p,num_dev,1);
if(err){
printk(KERN_ERR "LED: unable to add the device %d/n", err);
return err;
}
/*在/sys/class 下创建 led_test 目录*/
led_dev_class=class_create(THIS_MODULE,DEVICE_LIST);
if(IS_ERR(led_dev_class))
{
 err=PTR_ERR(led_dev_class);
goto unregister_cdev;
}

/*基于/sys/class/led_test 和/dev 下面创建 led_light 设备文件*/
temp=device_create(led_dev_class, NULL,num_dev, NULL, DEVICE_NODE);
if(IS_ERR(temp))
{
err=PTR_ERR(temp);
goto unregister_class;
}
return 0;

unregister_class:
class_destroy(led_dev_class);
unregister_cdev:
cdev_del(cdev_p);
return err;
}
/*模块的初始化*/
static int __init s5pv210_led_init(void)
{
int ret;
ret = s5pv210_led_ctrl_init();
if(ret)
{
printk(KERN_ERR "Apply: S5PV210_LED_init--Fail !!!/n");
return ret;
}
return 0;
}

/*模块的退出*/
static void __exit s5pv210_led_exit(void)
{
device_destroy(led_dev_class,num_dev);
class_destroy(led_dev_class);
cdev_del(cdev_p);
unregister_chrdev_region(num_dev,1);
}
MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("LED driver test");
module_init(s5pv210_led_init);
module_exit(s5pv210_led_exit);
```
### Makefile
```makefile
INSTALLDIR= /opt/tftp
ifneq ($(KERNELRELEASE),)
obj-m:=led_driver.o
else
KERNELDIR:=/opt/cross-compiler/kernel-embv210
PWD:=$(shell pwd)
default:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules
clean:
	rm -rf *.o *.order .*.cmd *.ko *.mod.c *.symvers
endif
install: led_driver.ko
	mkdir -p $(INSTALLDIR)
	cp --target-dir=$(INSTALLDIR) led_driver.ko
```
### 测试文件
```c led_test.c
// LED test programme
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#define DEVICE_NODE "/dev/led_light2"
int main(int argc,char **argv)
{
int fd,i,j;
unsigned char status;
unsigned char t;
/*打开设备节点*/
fd = open(DEVICE_NODE,O_RDWR);
if(fd == -1) 
{
printf("open device %s error \n",DEVICE_NODE);
return -1;
}
for(i=0;i<3;i++)
{
for(j=0;j<4;j++)
{
//依次点亮 LED1..LED4
t=(unsigned char)((1<<j)&0x0F);
write(fd,&t,sizeof(t));
if(read(fd,&status,1)!=0)
{
printf("led status:%0x\n",status);
}
sleep(1);
} }
close(fd);
return 0;
}
```