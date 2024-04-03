---
title: MATLAB 入门笔记
published: 2024-03-31
comment: false 
tags: [MATLAB]
category: MATLAB
---
## 概述

clear函数：清理工作区

clc函数：清理命令行窗口

pi表示圆周率，显化值为有限小数，内部计算时为高精度

内置函数：

format long：15位小数

format short：4位小数

script

## 数组

### 一般构造

`[3 5]`或 `[3,5]`数组，行向量，1 x n

`[3;5]`列向量，n x 1

### 等间距向量

`3:2:13`：3~13，间距2

关联函数：linspace(*first*,*last*,*number_of_elements*)

若 `x=[3 5]`,则 `x'=[3;5]`

注：使用 `:`不需要 `[]`转置时，使用括号如 `(3:2:13)'`

### 数组创建函数

- rand()

  eg: rand(3)：输出3x3的随机矩阵；rand(3,2)：输出3x2的随机矩阵
- zeros() 参数同上，输出所有值都为0的矩阵
- size()：获取矩阵大小
