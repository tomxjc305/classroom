# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于Three.js的3D旋转球名单抽取器项目。主要文件为`1.html`，实现了一个交互式的3D场景，用于随机抽取名单。

## 核心架构

### 3D渲染架构
- **Three.js场景系统**：使用WebGL渲染器 + 透视相机
- **球面分布算法**：采用斐波那契螺旋算法实现名字在球面的均匀分布
- **精灵系统**：每个名字使用Sprite对象，始终面向相机
- **纹理生成**：Canvas动态生成包含头像和姓名的贴图

### 数据管理
- `baseNames`数组：存储基础名单，支持通过shuffle算法扩展到任意数量
- `group`对象：Three.js Group容器，管理所有名字精灵的集合操作
- 内存管理：通过`dispose()`方法清理纹理资源

### 交互控制
- **鼠标拖拽**：pointerdown/pointermove/pointerup事件链实现3D旋转
- **双击暂停**：切换自动旋转状态
- **UI控件**：开始/停止/重置按钮 + 数量输入框

## 关键函数说明

### makeSphere(count)
使用斐波那契螺旋算法在球面上均匀分布指定数量的名字：
```javascript
const phi = Math.PI * (3 - Math.sqrt(5)); // 黄金角度
const y = 1 - (i / (count - 1)) * 2;       // Y轴从1到-1
```

### makeLabelTexture(text)
动态生成128x128的Canvas纹理，包含：
- 径向渐变背景圆形
- 阴影发光效果
- 居中显示的姓名文字
- 表情符号头像占位符

### fillNames(n) + shuffle(a)
名单扩展算法：通过洗牌算法循环复用基础名单达到指定数量

## 性能优化策略

- `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))`：限制像素比避免过度渲染
- `powerPreference:'high-performance'`：使用独立显卡
- `anisotropy`：纹理各向异性过滤提升质量
- 资源清理：每次重新生成球面时清理旧的材质和纹理

## 修改指南

### 更换名单
修改`baseNames`数组即可，支持中文姓名

### 调整外观
- 修改`--ui`CSS变量更改主题色
- 调整`makeLabelTexture`中的字体、颜色、大小
- 修改球面半径`R = 140`

### 添加功能
- 扩展UI控件在`footer`区域
- 新增交互事件监听器
- 修改动画参数`spin`控制旋转速度

## 浏览器兼容性
需要支持WebGL的现代浏览器，Three.js版本0.160.0