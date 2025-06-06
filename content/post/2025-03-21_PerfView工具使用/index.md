---
title: "PerfView工具使用"
slug: "工具之一"
date: 2025-03-21
image: image01.png
description: PerfView是微软提供的CPU和内存性能分析工具
tags:
  - tool
  - performance
---

# PerfView工具使用

[Github项目地址](https://github.com/microsoft/perfview?tab=readme-ov-file)

## 安装步骤

1. 下载最新版本PerfView
2. 解压缩到目标目录
3. 运行PerfView.exe

## 性能分析步骤

1. 启动PerfView
2. 选择"Collect" -> "Collect"
3. 配置收集参数：
   - 选择目标进程
   - 设置收集时间
   - 选择收集类型（CPU、内存等）
4. 点击"Start Collection"开始收集
5. 执行需要分析的场景
6. 点击"Stop Collection"结束收集

## 内存溢出检测步骤

1. 启动PerfView
2. 选择"Memory" -> "Take Heap Snapshot"
3. 选择目标进程
4. 点击"Take Snapshot"获取内存快照
5. 分析内存分配情况：
   - 查看大对象堆
   - 检查内存泄漏
   - 分析对象引用关系

## 结果分析方法

1. 打开收集的.etl文件
2. 使用"CPU Stacks"视图分析CPU使用情况
3. 使用"GCStats"视图分析内存使用情况
4. 使用"Events"视图查看详细事件
5. 使用"Processes"视图查看进程信息

## 注意事项

1. 收集时尽量减少其他程序运行
2. 适当延长收集时间以获得更准确结果
3. 分析时注意过滤无关信息
4. 保存分析结果以便后续比较
