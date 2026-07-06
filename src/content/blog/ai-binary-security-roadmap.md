
---
title: "我的 AI + 二进制安全学习路线"
description: "记录我为什么选择 AI 辅助二进制安全分析，以及后续学习计划。"
pubDate: 2026-07-06
tags: ["AI Security", "Binary Analysis", "Research"]
draft: false
---
# 我的 AI + 二进制安全学习路线

最近我准备把研究方向聚焦到 **AI 辅助二进制安全分析**。

## 为什么选择这个方向

二进制安全关注真实程序中的底层漏洞、逆向分析、漏洞检测和系统安全问题。

深度学习和大语言模型可以帮助提升分析效率，例如：

- 函数摘要
- 危险 API 识别
- 反编译代码解释
- 漏洞风险判断
- 逆向分析报告生成

## 我计划学习的内容

- C / C++ 内存模型
- x86-64 汇编
- ELF 文件格式
- Ghidra / angr
- Fuzzing 和漏洞检测
- PyTorch 和深度学习基础
- LLM 辅助逆向工程

## 项目目标

我计划做一个 **AI-assisted Binary Analysis System**，支持上传二进制文件，自动提取函数、字符串、危险 API、调用关系，并结合 LLM 生成函数摘要和风险分析报告。
