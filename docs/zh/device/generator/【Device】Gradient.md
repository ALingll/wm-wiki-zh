---
title: Gradient
layout: home
has_toc: true
parent: 生成器
permalink: /zh/device/generator/gradient
---
# Constant - 常量场

> Fill terrain with a constant height value.
> 以恒定高度值填充地形。

**Constant** 生成一个具有恒定高度的水平面地形。当提供遮罩输入时，Constant 将输出常量场与遮罩场相乘得到结果。

---
## Ports：
- **Outputs**
	- **\<主输出\>Primary Output (Bitmap)**
- **Params**
	- **\<控制端口\>Control Port (DeviceController)**
	- **\<置位输入\>Placement Input (Placement, Distortion)**
	- **\<颜色\>color (color)**
- **Mask**

---
## Placement & Transform:
- **Placement**

---
## Parameters:
- **{%include inline-ver-tag.html desc="[S]" ver="AP 起" %} slider : \<高度\>Height : Elevation**   
	指定生成水平面地形的高度值。

---
## 历史版本

- Legacy
- HR：支持空间参数；