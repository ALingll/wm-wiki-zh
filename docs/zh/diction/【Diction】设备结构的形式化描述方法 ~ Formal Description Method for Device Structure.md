此规范用于描述`[Device]`标签下页面的叙述结构和语法细节。我们期望定义一种严谨、直观、可复制的描述方法，用于描述设备的各种特性。

**如非必要，不再额外对 WM2.x 版本的参数进行说明。**

**当使用 MarkDown 格式撰写结构化声明时，请不要忘了使用反斜杠来转义特殊字符，例如方括号和尖括号。**

设备描述页面分为以下几个部分：
- 名称：设备的名称；
- 简述：对设备的背景、功能、用法的简要描述；
- 端口（Ports）：对设备的输入、输出、参数、遮罩端口的描述；
- 置位和变换（Placement & Transform）：对属性面板中Placement & Transform栏目的描述，如果该设备不包含此栏目则省略；
- 参数（Parameters）：对属性面板中设备参数栏目的描述；所有参数按应最新版本的WM参数面板顺序进行排序；**对仅移动参数位置（包括改变参数顺序、打组方式）、不改变参数效果的更新，不使用版本注释。**
- 拓展内容：任意数量的拓展内容，可以包括参考文献等。

各部分之间需要使用分割线；名称使用一级标题，其他部分使用四级标题；形如：
```markdowm
# Device Name - 设备名称
Some Description...
---
#### Ports
contents...
---
#### Placement & Transform
contents...
---
#### Parameters
contents...
---
#### Other Extensive Parts1
contents...
---
#### Other Extensive Parts2
contents...
....
```

设备页面规范的一个典例是 Advanced Perlin 页面，参见[此处](https://github.com/ALingll/wm-wiki-zh/wiki/%E3%80%90Device%E3%80%91Advanced-Perlin)

---
## 名称

设备的名称，使用一级标题，其格式为`{en-title}+\space+\sub+\space+{title}`。包含设备的页面语言名称和英文名称，其中英文名称在前，形如：`Advanced Perlin - 高级柏林噪波`。当页面语言为英文时，省略页面语言名称。

其巴科斯范式语法描述为：
```
<DeviceName> ::= <EnTitle> <Space> "-" <Space> <Title>
               | <EnTitle>

<EnTitle> ::= <EnWord> | <EnWord> <Space> <EnTitle>

<Title> ::= <WordChars> | <WordChars> <Space> <Title>

<EnWord> ::= <EnLetter> | <EnLetter> <EnWord>

<WordChars> ::= <ChineseChar> | <Digit> | <Letter> | <ChineseChar> <WordChars> | <Digit> <WordChars> | <Letter> <WordChars>

<Letter> ::= <CapitalLetter> | <LowerLetter>

<CapitalLetter> ::= "A" | "B" | ... | "Z"

<LowerLetter> ::= "a" | "b" | ... | "z"

<Digit> ::= "0" | "1" | ... | "9"

<ChineseChar> ::= (任意中文字符)

<Space> ::= " "
```

---
## 端口

端口栏目描述设备的外部结构，使用分级列表的方式分别列举出设备的输入、输出、参数、遮罩端口，形如（注意，markdown代码中需要使用`\[`和`\]`来对方括号进行转义）：
```markdowm
#### **Ports：**
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
```

**端口条目**指的是端口列表的一个叶子节点，对应设备的一个端口。端口条目按种类分列于四个一级列表项（**Inputs**，**Outputs**，**Params**，**Mask**）下。四个一级列表项均使用粗体。端口条目由**端口声明**和**端口描述**组成，其中，端口声明是端口性质、类型的结构化声明描述，使用粗体。

其中 **Mask** 无需包含子项，省略 **Mask** 代表该设备没有遮罩端口。 

端口声明的格式为`[{properties}]+\space+<{name}>+{en-name}+\space+(type-ls)`，其中：
- `[{properties}]` 是零个或一个或多个使用方括号包裹属性名，用于表示端口的性质，可选的值有：
	- `[Opt]`：表示该端口可选，如果用于修饰输入端口，则表示该输入对于该设备不是必须的；如果用于输出端口则表示该端口不一定有输出/输出可能为空值。
- `<{name}>`是使用尖括号包裹的端口名称的页面语言名称，如果页面语言为英文，则省略;
- `{en-name}`是端口名称原生版本；
- `(type-ls)`是使用圆括号包裹的一个或多个使用`,+\space`间隔的类型名称，表面该输入/输出端口可能接受/输出这些类型的数据，其可选值参见[数据类型](链接确实)

例如：`[Opt] <形状导量>Shaping Guide (Heightfield)`，`<置位输入>Placement Input (Placement, Distortion)`

其巴科斯范式语法描述为：
```
<PortDecl> ::= <Properties> <NamePart> <EnName> <Space> <TypeList>
             | <Properties> <EnName> <Space> <TypeList>

<Properties> ::= <Property> | <Property> <Properties>

<Property> ::= "[" <PropertyName> "]"

<PropertyName> ::= "Opt" | ...

<NamePart> ::= "<" <Title> ">"

<Title> ::= <WordChars> | <WordChars> <Space> <Title>

<EnName> ::= <EnWord> | <EnWord> <Space> <EnName>

<TypeList> ::= "(" <Type> | <Type> <TypeSep> <TypeList> ")"

<TypeSep> ::= "," <Space>

<Type> ::= <EnWord> | <EnWord> <Space> <Type>

<EnWord> ::= <EnLetter> | <EnLetter> <EnWord>

<WordChars> ::= <ChineseChar> | <Digit> | <Letter> 
               | <ChineseChar> <WordChars> 
               | <Digit> <WordChars> 
               | <Letter> <WordChars>

<Letter> ::= <CapitalLetter> | <LowerLetter>

<CapitalLetter> ::= "A" | "B" | ... | "Z"

<LowerLetter> ::= "a" | "b" | ... | "z"

<Digit> ::= "0" | "1" | ... | "9"

<ChineseChar> ::= (任意中文字符)

<Space> ::= " "
```

特殊端口：
- 所有设备的 **Control Port** 和 **Placement Input** 都具有相似的性质，故只需复制并列举它们，当设备不接受此类端口时，只需省略它们。
- 由参数定义的端口，它们的性质和存在与否与设备本身的接受的参数直接相关，描述为`[...] Parameters`

---
## 置位和变换

所有设备的置位和变换栏目都相似，但不是所有设备都同时支持**Scope**，**Position**，**Rotation**三个操作。当该设备不支持某些操作时，只需省略它们，形如:
```markdown
#### **Placement & Transform:**
- **Scope**
- **Position**
- **Rotation**
```

---
## 参数

参数是通过属性面板或外部参数接口输入控制的设备的属性。使用分级列表的方式列举出设备的所有参数，其中：
- 分级列表的每个节点代表一个**项**；
- 每个项包括**声明**和**描述**，其中声明是是项性质、类型的结构化声明描述，使用粗体表示；根据类别的不同，项分为：
	- **参数项**：与设备参数一一对应；
	- **枚举项**：作为类型为`dropdown`的参数项的子节点，用于列举其每种选项的作用；
	- **组**：包含一个或多个参数项作为其子节点。

#### 参数项

参数项形如`[A U] checkbox : <自定义分型...>Customize fractal... : bool`，格式为`[{properties}]+\space+{ui-type}+\space+":"+\space+<{lang-name}>+{name}+\space+":"+\space+{type-decl}+\space+":"+\space+{dependence}`，其中：
- `properties`是由方括号包裹、由空格间隔的一个或多个属性声明，表示该参数的性质，可选项包括：
	- `[A]`：该参数再Ui中显示，仅当设备启用高级参数设置；
	- `[U]`：该参数无法通过外部参数接口控制；
	- `[S]`：该参数接受空间参数；
	- `[F]`：该参数为非常规类型参数，无法适用一般参数项声明格式，应分别讨论；
- `ui-type`是参数的Ui类型，可选项包括：
	- `slider`
	- `dropdown`
	- `checkbox`
	- `field`
	- `action`
	- ......
- `lang-name`是由尖括号包裹的页面语言版本的参数名称，当页面语言为英语时省略不写；
- `name`是参数的名称；
- `type-decl`是参数类型的声明，包括参数的类型与取值范围，包括三种情况：
	- **数值类型**：例如`int`，`enum`，`float`等，再参数类型后紧跟由花括号包裹的取值范围，例如`float{0~1}`；
	- **量纲类型**：例如`Distance`，在上述条件下，取值范围需要包括量纲，例如`Distance{1m~1024km}`；
	- **布尔类型**：`bool`，省略取值范围；
	- **信号类型**：`action`，省略取值范围；
- `dependence`是可选的，表示一个参数可见性/是否启用的依赖关系，形如`visible(Customize fractal...==true)`，包括**依赖类型**，**依赖于**，**条件**和**目标值**。

其巴科斯范式语法描述为：
```
<ParamDecl> ::= <Properties> <Space> <UiType> <Space> ":" <Space> <LangName> <Name> <Space> ":" <Space> <TypeDecl> <Dependency>
              | <Properties> <Space> <UiType> <Space> ":" <Space> <Name> <Space> ":" <Space> <TypeDecl> <Dependency>

<Dependency> ::= "" | ":" <Dependence>

<Dependence> ::= <DenendencyType> "(" <Name> <JudgeType> <TargetValue> ")"

<DenendencyType> ::= "visible" | "enable"

<JudgeType> ::= "==" | "!=" | ">" | "<" | ">=" | "<="

<TargetValue> ::= <Number> | <Value>

<Properties> ::= <Property> | <Property> <Space> <Properties>

<Property> ::= "[" <PropertyFlag> "]"

<PropertyFlag> ::= "A" | "U" | "S" | "F" | ...

<UiType> ::= "slider" | "dropdown" | "checkbox" | "field" | "action" | ...

<LangName> ::= "<" <Title> ">"

<Title> ::= <WordChars> | <WordChars> <Space> <Title>

<Name> ::= <UnicodeChars>

<TypeDecl> ::= <NumType> | <DimType> | <BoolType> | <ActionType>

<NumType> ::= <BaseNumType> "{" <Number> "~" <Number> "}"

<BaseNumType> ::= "int" | "enum" | "float" | ... (* 可扩展 *)

<DimType> ::= <DimensionType> <Range>

<DimensionType> ::= "Distance" | ... (* 可扩展 *)

<BoolType> ::= "bool"

<ActionType> ::= "action"

<Range> ::= "{" <RangeExpr> "}"

<RangeExpr> ::= <Value> "~" <Value>

<Value> ::= <Number> | <Number><Unit>

<Number> ::= <Digit> | <Digit> <Number>

<Unit> ::= "m" | "km" | ...   (* 可扩展 *)

<WordChars> ::= <ChineseChar> | <Digit> | <Letter> 
               | <ChineseChar> <WordChars>
               | <Digit> <WordChars>
               | <Letter> <WordChars>

<UnicodeChars> ::= (任意 Unicode 字符序列)

<Identifier> ::= <Letter> | <Letter> <Identifier>

<CapitalLetter> ::= "A" | "B" | ... | "Z"

<LowerLetter> ::= "a" | "b" | ... | "z"

<Digit> ::= "0" | "1" | ... | "9"

<ChineseChar> ::= (任意中文字符)

<Space> ::= " "
```

#### 枚举项

枚举项用于作为类型为`dropdown`的参数项的子节点，用于列举其每种选项的作用，形如`enum(0) :  <信号值>Signal Level`，其巴科斯范式语法描述为：
```
<EnumItem> ::= "enum" <Ordinal> ":" <LangName> <Name>

<Ordinal> ::= "<" <Number> ">"
```

#### 组

组是参数的容器，包含一个或多个参数项作为其子节点，并且可以是匿名的，其巴科斯范式语法描述为：
```
<Group> ::= <Properties> <Space> "group" <Space> <GroupNamePart> ":" <Space> <TypeDecl> <Dependency>

<GroupNamePart> ::= "" | ":" <Space> <Name> <Space> | ":" <Space> <LangName> <Name> <Space>
```

#### 非常规参数

**列表**：形如`[U] table : <阶>Octave | <强度>Strength | <样式>Style : int | int | enum`；