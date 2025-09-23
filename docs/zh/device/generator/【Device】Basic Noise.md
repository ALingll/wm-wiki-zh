---
title: Basic Noise
layout: home
has_toc: true
parent: 生成器
permalink: /zh/device/generator/basic-noise
---
# Basic Noise - 基础噪波

> Create noise texture using perlin noise.
> 使用柏林噪波创建的噪波纹理。

**Basic Noise** 是一个基于柏林噪声的轻量的柏林噪波生成器，它拥有较为简洁的参数。关于柏林噪波和分形布朗运动，参见 [Advanced Perlin]({{site.baseurl}}/zh/device/generator/advanced-perlin)。它的可定制性弱于 Advanced Perlin。

{%include inline-ver-tag.html desc="与 Advanced Perlin 相比，它的绝大多数参数都支持空间参数，这使得用户可以通过在参数端口输入空间参数来使生成的噪声具有高度可控的空间局部特征。" ver="AP 起" %}

---
## Ports：
- **Inputs**
	- {% include ver-tag.html 
		title="[Opt] <固体噪声>Solid Noise (Heightfield)" 
		desc="利用输入场作为z平面对三维噪声进行采样取值，得到连续且具有输入场特征的局部差异性的噪波。" 
		ver="AP 起" %}
- **Outputs**
	- **\<主输出\>Primary Output (Heightfield)**
- **Params**
	- **\<控制端口\>Control Port (DeviceController)**
	- **\<置位输入\>Placement Input (Placement, Distortion)**
	- **\[...\] Parameters**
- **Mask**

---
## Placement & Transform:
- **Placement**
- **Position**
- **Rotation**

---
## Parameters:
- **{% include inline-ver-tag.html desc="[P]" ver="AP 前"%}{%include inline-ver-tag.html desc="[P S]" ver="AP 起" %} slider : \<特征尺度\>Feature Scale : Distance{1m~1024km}**   
	控制fBm过程中顶级噪波特征的空间尺度，数值越大，生成的地形越宽广，纹理越粗旷。
- **group : \<分型特征\>Fractal Character**
	- **dropdown : \<样式\>Style : enum{0~8}**  
		选择基础噪波的结构样式，影响地形形态的整体风格与细节特征。
		- **enum(0) : \<基础\>Basic**  
			经典的 Perlin Noise。生成的地形看起来有点像非常崎岖不平的锯齿状山地地形。
		- **enum(1) : \<脊状\>Ridged**  
			尖锐的不连续性遍布整个地形的各个尺度；看起来像地形中的山脊和脊椎。
		- **enum(2) : \<波涛\>Billowy**  
			与【Ridged】相反，产生的地形具有块状的外观，整个地形带有尖锐的凹陷折痕。
	- **{%include inline-ver-tag.html desc="[S]" ver="AP 起" %} slider : \<持续性\>Persistence : float{0~0.75}**  
		控制fBm过程中每层噪波振幅的衰减程度，值越高，细节保持越多，层次越明显。
	- **\[A\] slider : \<阶数\>Octaves : int{0~24}**  
		控制fBm过程中噪波叠加的层数，层数越多，地形层次越丰富；置0时将自动分配合适的阶数。
	- **\[A\] field : \<种子\>Seed : int**  
		控制生成噪波的随机性，更改随机种子将创建与当前种子具有不同分布但特征相似的新地形。
{% capture vertical_field %}
- **group : \<垂直尺度\>Vertical Scale**
	- **dropdown : enum{0~2}**
	    选择垂直尺度的处理方式。
	    - **enum(0) : \<全范围\>Use Full Range**
	        使用完整的输入范围值域。
	    - **enum(1) : \<指定范围\>Specific Range**
	        使用指定的范围。
	    - **enum(2) : \<指定坡度\>Specific Gradient**
	        使用指定的坡度范围。
{% endcapture %}
{% include field-ver-tag.html field=vertical_field ver="WM 3016" %}
- **group : \<高程控制\>Evaluation Control**
	- **{%include inline-ver-tag.html desc="[S]" ver="AP 起" %} slider : \<中位高程\>Middle elevation : Elevation**  
		设置噪波生成高程的中位数，更改此值将生成主要位于较低或较高高程的地形。
	- **{%include inline-ver-tag.html desc="[S]" ver="AP 起" %} slider : \<陡峭度\>Steepness : float{0.005~0.995}**  
		控制噪波地形起伏的陡峭程度，低值会产生更平坦的地形；高值将产生陡峭的悬崖。
    {% capture vertical_field %}
	- **dropdown : \<映射到\>Scale to : enum{0~2}**   
		选择垂直尺度的处理方式。
	    - **enum(0) : \<全范围\>Use Full Range**   
	        使用完整的输入范围值域。
	    - **enum(1) : \<指定范围\>Specific Range**   
	        使用指定的范围。
	    - **enum(2) : \<特征尺度 & 坡度\>Feature Scale & Slope**   
	        使用指定的坡度范围。
	- **\[S\] slider : \<高程下界\>Elevation Base : Elevation : visible(Scale to=1|2)**   
		指定输出高度场的高程最小值。
	- **\[S\] slider : \<高程上界\>Elevation Max : Elevation : visible(Scale to=1)**   
		指定输出高度场的高程最大值（注：当作为空间参数输入时可与 Angle 共同作用）。
	- **\[S\] slider : \<坡度\>Slope : Angle{0°~45°} : visible(Scale to=2)**   
		指定输出高度场的坡度值（注：当作为空间参数输入时可与 Elevation Max 共同作用）。
{% endcapture %}
{% include field-ver-tag.html field=vertical_field ver="AP 起"%}

---
## 历史版本

- WM 2.x
- WM 3016：新的，简洁的柏林噪波生成器；
- AP：添加了固体噪声输入和空间参数；
- HR：改进了性能；