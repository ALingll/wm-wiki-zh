---
title: Color
layout: home
has_toc: true
parent: 生成器
permalink: /zh/device/generator/color
---
# Color - 颜色

> Create a RGB texture that is a single solid-color.
> 创建纯色的 RGB 纹理。

**Color** 生成一张纯色的纹理图片，通常作为制作地形纹理的起点。注意，它的输出类型为图片类型 Bitmap，而不是参数类型 color。

当提供遮罩输入时，Color会将输出 Bitmap 的三个通道分别与遮罩相乘，这表现为，遮罩中值越低的位置，生成的纹理颜色越黑。

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
- **color-picker : color**   
	定义生成单色纹理的颜色，可以以HSL，HSB，RGB方式取色；支持屏幕取色；支持输入Hex；会自动计算物理反照率。