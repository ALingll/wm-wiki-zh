---
title: Advanced Perlin
has_toc: true
parent: 生成器
layout: home
permalink: /zh/device/generator/advanced-perlin
---
# Advanced Perlin - 高级柏林噪波

> A highly configurable perlin noise generator.
> 一个高度可配置的柏林噪波生成器。

**Advanced Perlin** 是 WorldMachine 中一个高度可定制的分形噪声生成器，作为基础地形（base terrain）的常用起点。该设备基于 Perlin 与分形噪声的原理，将多个尺度（octaves）的噪声层叠合成最终高度场，并允许对每一层的类型与合成方式进行细粒度控制，从而生成从大尺度地形到细微纹理的多种地貌形式。Advanced Perlin 不仅提供传统的尺度/持久性控制，还支持外部“形状引导”（Shape Guide）与基于场的空间参数控制，以便在空间上对噪声特性进行局部调节。

设备的输入端口包含遮罩输入（用于按区域限定噪声影响）、以及可选的 **Shape Guide** 输入（用于控制噪声的宏观形状）。同时，高级参数还支持通过参数端口或引导贴图逐像素控制持久性（persistence）、增益（gain）、自定义频谱（fractal profile）等，从而实现更加异质化的噪声效果。实践中，Shape Guide 常用于定义大尺度“形状”（例如岛屿轮廓、山脊走向），而噪声自身的设置则控制细节与粗糙度。

Advanced Perlin 常被用作“场景第一步”：先用较低频（大尺度）设置或 Shape Guide 构建总体地形形态，再接入 Erosion（侵蚀）等滤镜以产生更真实的地貌演化效果；随后可回到本设备或其它噪声/混合设备加入局部细节。构建大型或可平铺的高度场时，应注意 Feature Scale 与项目尺寸的匹配，以及后续输出与导出（tiling / border handling）策略。社区实践表明，将 Shape Guide 与 Shapes/Mask 配合可以在控制地形位置与形态方面获得最直接、可控的结果。

---
##  分形布朗运动（fBm）

分形布朗运动（fractional Brownian motion, fBm）是 Mandelbrot 和 Ness 于1968年提出的随机过程模型$^{[1]}$，是一种通过迭加多层次噪波函数来生成的分形结构。它能够在视觉上呈现出自然界中常见的分形特征，例如山脉、云层和地形等。在1983年，Mandelbrot 意识到 fBm 过程的输出在视觉上与山脉崎岖轮廓相似，并首次尝试使用 fBm 创建山脉、大陆、崎岖的海岸线等自然景观$^{[2]}$。

fBm 的基本构造方式是将多个频率递增、振幅递减的噪声函数按层（octaves）叠加。其一般形式为：

$$
\text{fBm}(\mathbf{x}) = \sum_{i=0}^{N-1} \; \text{noise}(2^i \mathbf{x}) \cdot \text{gain}^i
$$

其中：
- $N$ 表示叠加的层数（octaves）。
- $\mathbf{x}$ 表示输入坐标。
- $\text{noise}(\cdot)$ 表示基础噪声函数，例如 Perlin 噪声。
- $2^i$ 体现频率逐层倍增。
- $\text{gain} \in (0,1)$ 为每层的振幅衰减因子。

通过调整 `lacunarity`（频率增长因子）与 `gain`（振幅衰减因子），fBm 可以生成从光滑到粗糙的不同分形外观。典型情况下，`lacunarity` 取值大于 1，用于控制每层噪声的空间频率增加速率；而 `gain` 用于控制整体的粗糙度与细节保留程度。

在 Advanced Perlin 设备中，fBm 被用于生成具有多层次的细节的噪波，它能够在保持整体连贯性的同时，引入逐层细化的纹理细节。

---
## **Ports：**
- **Inputs**
	- **\[Opt\] \<形状导量\>Shaping Guide (Heightfield)**
	- **\[Opt\] \<畸变导量\>Distortion Guide (Heightfield)**
	- **\[Opt\] \<持续导量\>Persistence Guide (Heightfield)**
- **Outputs**
	- **\<主输出\>Primary Output (Heightfield)**
- **Params**
	- **\<控制端口\>Control Port (DeviceController)**
	- **\<置位输入\>Placement Input (Placement, Distortion)**
	- **\[...\] Parameters**
- **Mask**

---
## **Placement & Transform:**
- **Scope**
- **Position**
- **Rotation**

---
## **Parameters:**
- **\[P\] slider : \<特征尺度\>Feature Scale : Distance{1m~1024km}**   
	控制fBm过程中顶级噪波特征的空间尺度，数值越大，生成的地形越宽广，纹理越粗旷。
- **dropdown : \<样式\>Style : enum{0~8}**  
	选择基础噪波的结构样式，影响地形形态的整体风格与细节特征。
	- **enum(0) : \<基础\>Basic**  
		经典的 Perlin Noise。生成的地形看起来有点像非常崎岖不平的锯齿状山地地形。
	- **enum(1) : \<脊状\>Ridged**  
		尖锐的不连续性遍布整个地形的各个尺度；看起来像地形中的山脊和脊椎。
	- **enum(2) : \<波涛\>Billowy**  
		与【Ridged】相反，产生的地形具有块状的外观，整个地形带有尖锐的凹陷折痕。。
	- **enum(3) : \<平滑脊状\>Smooth Ridged**  
		类似于【Ridged】样式，但具有平滑的脊顶部，而不是尖锐的脊。
	- **enum(4) : \<平滑波涛\>Smooth Billowy**  
		具有块状形状，整个形状都有光滑的折痕。
	- **enum(5) : \<尖锐脊状\>Sharp Ridged**  
		类似于【Ridged】样式，但比常规【Ridged】样式具有更陡峭的悬崖。
	- **enum(6) : \<中部平台\>Flat Middle**  
		中高程区域被拉平，在山腰处形成腰带状平台。
	- **enum(7) : \<阶梯\>Terraced**  
		形成具有阶梯状断层的起伏。
	- **enum(8) : \<Stephen特调1号\>Stephen's Choice 1**  
		由 World Machine 的作者 Stephen Schmitt 创建的多重分形变体。它具有大尺度的脊，很像脊状变体，但在较小的尺度上，它具有波涛汹涌变体的大部分特征。
- **slider : \<持续性\>Persistence : float{0~0.75}**  
	控制fBm过程中每层噪波振幅的衰减程度，值越高，细节保持越多，层次越明显。
- **\[A\] slider : \<空隙度\>Lacunarity : float{1.4~6.0}**  
	控制fBm过程中每层噪波频率的增长倍率，值越大，纹理更细密。
- **\[A\] slider : \<阶数\>Octaves : int{0~24}**  
	控制fBm过程中噪波叠加的层数，层数越多，地形层次越丰富；置0时将自动分配合适的阶数。
- **\[A\] field : \<种子\>Seed : int**  
	控制生成噪波的随机性，更改随机种子将创建与当前种子具有不同分布但特征相似的新地形。
- **slider : \<陡峭度\>Steepness : float{0.005~0.995}**  
	控制噪波地形起伏的陡峭程度，低值会产生更平坦的地形；高值将产生陡峭的悬崖。
- **slider : \<中位高程\>Middle elevation : Elevation**  
	设置噪波生成高程的中位数，更改此值将生成主要位于较低或较高高程的地形。
- **\[A\] checkbox :  \<高程控制\>Evaluation control : bool**  
	启用后，允许用户指定噪波被钳位至特定高程范围。
- **\[A\] group : visible(Evaluation control\==True)**  
	- **field :  \<低限位\>Lowest : Elevation**  
		设置噪波影响的最低高程。
	- **field :  \<高限位\>Highest : Elevation**  
		设置噪波影响的最高高程。
- **\[A\] checkbox :  \<引导输入\>Guide inputs : bool**  
	启用后，将允许用户指定输入导量对噪波的形状、畸变、持续性进行引导的强度。
- **\[A\] group : visible(Guide inputs\==frue)**  
	- **slider : \<形状引导渐入等级\>Shapeguide lead-in level : int{1~4}**  
		仅当形状导量输入合法时才生效。允许您设置形状导量接管的噪声阶数。较高的值仅允许较小的分形变化通过。
	- **slider : \<畸变引导等级\>Distortion guide level : float{0~1}**  
		仅当畸变导量输入合法时才生效。扭曲可以产生一些狂野的地形效果，因为特征会像太妃糖一样被拉扯和扭曲。值越高，产生的畸变就越大。
	- **slider : \<持续引导等级\>Persistence guide level : float{0~1}**  
		仅当持续导量输入合法时才生效。控制持续导量控制地形局部持续性的能力，值越高，输入值会导致的持久性差异更大。
- **\[A\] checkbox : \<多尺度控制\>Multiscale control : bool**  
	启用后，将允许用户设置多尺度分型参数。多尺度分形是在不同尺度上表现出不同特征的分形。通常，这是基于高程、位置或其他因素。这些往往会产生更复杂或更复杂的地形。这个概念从语言表述上来看有些抽象，因此其使用的最佳指南是实践。
- **\[A\] group : visible(Multiscale control\==true)**  
	- **slider :  \<活跃度\>Activity : float{0~1}**  
		设置应用多尺度效果的应用强度。
	- **slider :  \<偏移量\>Offset : float{0~1}**  
		 确定分形行为在哪个高度开始更改其特征，每个变体在此处使用值的方式略有不同。
	- **slider :  \<增幅\>Gain : float{0~1}**  
		控制分形的详细区域与平滑区域的相对强度。低值非常平滑，而高值则更快地带来细节性质。
	- **slider :  \<起始阶数\>Lead-in octave : int{0~8}**  
		设置噪声的前几阶不受多尺度行为的影响。这样，您可以确保地形的整体特征不会因多尺度行为而发生意外的改变。
	- **dropdown :  \<类型\>Type : enum{0~4}**  
		有几种不同的多尺度组合方法；最好的指导是实践，因为改变它们通常会获得意想不到的结果。
		- **enum(0) :  \<信号值\>Signal Level**  
			多尺度特性取决于当前阶的噪波的强度。
		- **enum(1) :  \<高程值\>Elevation**  
			多尺度特征根据当前点的高程而变化
		- **enum(2) :  \<双向信号值\>Bi-Signal Level**  
			多尺度活动在当前当前阶的噪波的值域的两端同时进行
		- **enum(3) :  \<双向高程值\>Bi-Elevation**  
			多尺度活动在高程的两端都开始
		- **enum(4) :  \<经典\>Classical**  
			一种类似于 Signal Level 的多尺度方法，用于 WorldMachine 的早期版本
- **\[A U\] checkbox : \<自定义分型...\>Customize fractal... : bool**  
	启用后，将允许用户自定义分型。在正常的分形噪波类型中，噪波的每一阶都使用相同的基函数来定义它。自定义频谱（Customize Fractal Profile）选项允许你为每一阶分配一个自定义类型；这可以生成一些非常独特的地形特征。自定义阶是从最低级别 （最大特征大小） 到最高 （最小） 创建的。
- **\[A\] group : visible(Customize fractal...\==true)**  
	- **\[U\] table : \<阶\>Octave | \<强度\>Strength | \<样式\>Style : int | int | enum**  
		一个显示所有自定义阶的表格，其中每一行表示一个自定义阶，他们通过下述按钮添加或删除；每一行有三列：【Octave】表示当前阶序，不可编辑；【Strength】表示当前阶的作用强度，高值将为当前阶施加较高的影响程度；【Style】设置当前阶的类型。
	- **\[U\] button : \<添加阶\>+Octave : Action**  
		在任何现有噪波上添加新的自定义阶。
	- **\[U\] button : \<删除阶\>-Octave : Action**  
		从堆栈中删除最上面的阶。
	- **\[U\] button : \<全部清除\>Clear All : Action**  
		移除所有自定义噪波阶。

---
## 缺陷性报告

### 放射状伪影

fBm 本质上对坐标是敏感的，当频率成倍扩展时，原点往往是所有频率层的“对齐点”。因此从原点向外的方向容易积累各层次的微弱规律性，叠加后表现为放射状的“星形伪影”。当不同 octave 完全对齐且使用相同 seed，会产生规则化的伪影，常规实现通常通过对每层偏移/重播种以打破对齐。

<div style="text-align: center; max-width: 100%;">
  <img src="{{site.baseurl}}/assets/images/advanced_perlin.artifacts.png" alt="图片描述" style="width: min(33.333%, 512px); height: auto; display: block; margin: 0 auto;">
  <div style="margin-top: 0.5em; font-size: 0.95em; color: #333;">fBm 产生的放射状伪影</div>
</div>
要产生上述图片，参考如下设置：
```
Advanced Perlin{
	Feature Scale = 5km,
	Persistence = 0.59,
	Lacunarity = 1.4,
	Octaves = 15
}
```
在WM中，要避免这种伪影，一种方案是使用 Placement & Transform 中的 Position，将噪波偏移设置为一个较大的值，使得放射中心点远离采样区域。也可以适当调高 Lacunarity 的值，当 Lacunarity 较高时，该伪影不明显。

---
## 画廊

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 1em;">

  <div style="flex: 0 0 48%; text-align: center; margin-bottom: 1em;">
    <img src="{{site.baseurl}}/assets/images/advanced_perlin.gallery1.png" alt="图片1" style="width: 100%; max-width: 512px; height: auto;">
  </div>

  <div style="flex: 0 0 48%; text-align: center; margin-bottom: 1em;">
    <img src="{{site.baseurl}}/assets/images/advanced_perlin.gallery2.png" alt="图片2" style="width: 100%; max-width: 512px; height: auto;">
  </div>

  <div style="flex: 0 0 48%; text-align: center; margin-bottom: 1em;">
    <img src="{{site.baseurl}}/assets/images/advanced_perlin.gallery3.png" alt="图片3" style="width: 100%; max-width: 512px; height: auto;">
  </div>

  <div style="flex: 0 0 48%; text-align: center; margin-bottom: 1em;">
    <img src="{{site.baseurl}}/assets/images/advanced_perlin.gallery4.png" alt="图片4" style="width: 100%; max-width: 512px; height: auto;">
  </div>
</div>


---
## 参考文献

1. Mandelbrot, Benoît B.; Van Ness, John W. *Fractional Brownian motions, fractional noises and applications*. SIAM Review, 10(4), 422–437, 1968.
2. Mandelbrot, Benoît B. ''The Fractal Geometry of Nature''. W. H. Freeman and Company, 1982.


