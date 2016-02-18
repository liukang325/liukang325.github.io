---
layout: post
title: 状态机编程技巧：状态表与函数表
category: 技术
tags: 设计模式
keywords: 
description: 
---

没有swith case那样可以直接能通过程序代码看到各 状态 的跳转与 状态机 的执行步骤，但这种 状态表与函数表 实现的 状态机。代码简洁，无遗漏状态，经典！

简单实例：(C语言)

```
#include <stdio.h>  
  
char str[128] = "   ./a.out 100   200   ";  
int argc;  
char * argv[16];  
  
int i = 0;  
void act_save(void)  
{  
    argv[argc++] = str + i;  
}  
  
void act_end(void)  
{  
    str[i] = '\0';  
}  
  
void act_null(void)  
{  
  
}  
  
int state_trans_table[2][2] =  
{  
    { 0, 1 },  
    { 0, 1 }  
};  
  
  
void (*act_table[2][2])(void) =  
{  
    { act_null, act_save },  
    { act_end, act_null }  
};  
  
int get_input_type(char c)  
{  
    if (str[i] == ' ')  
        return 0;  
      
    if (str[i] != ' ')  
        return 1;  
  
    return 0;  
}  
  
void fsm(void)  
{  
    int state = 0;  
    int input = 0;  
  
    while (str[i])  
    {  
        /* get input char type */  
        input = get_input_type(str[i]);  
  
        /* call action */  
        act_table[state][input]();  
          
        /* transfer to next state */  
        state = state_trans_table[state][input];  
  
        /* get next input */  
        i++;  
    }  
  
    return;  
}  
  
int main(void)  
{  
    int i = 0;  
  
    fsm();  
  
    printf("argc = %d \n", argc);  
    for (i = 0; i < argc; i++)  
        printf("argv[%d] = %s \n", i, argv[i]);  
  
    return 0;  
}  
```