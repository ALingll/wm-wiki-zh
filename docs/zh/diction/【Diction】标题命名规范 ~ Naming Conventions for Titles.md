标题命名规范旨在统一Wiki中页面命名方式，以使读者能够在搜索栏中准确而方便地定位内容。

**页面语言**指的是当前页面用于叙述的自然语言，就本页面而言，页面语言为简体中文。

页面名称采用`[tag]+{title}+\space+"~"+\space+{en-title}`。其中，**Tag** 是使用方括号包围的首字母大写的简短词汇，其目的是明确本页面所属的分类或内容概述。为了保证分类简介明了，应当使用本页面中给出的  Tag，尽量不要自拟 Tag。允许多个 Tag，其中主标签位于子标签的前面，形如`[MainTag][SubTag] 这是标题 - This Is A Title`。**仅当 Tag 用于文件名时，使用全角方括号**。`title`是页面标题的页面语言版本，`en-title`则是其英文版本，当页面语言为英文时，应当仅包含`en-title`。特别地，`[Device]`标签下的所有页面标题都应当仅包含`en-title`。

文章标题应遵循英文文章标题命名方式，尽量简短而准确。应当使用短语而不是句子。对于`[Device]`标签下的页面，则应当使用设备的名称作为标题。

我们引入如下 Tag：
- `[Navigation]`：用于组织目录、索引和页面跳转，帮助读者快速定位内容。
- `[Concept]`：阐明各种基本概念以及专业名词。
- `[Device]`：介绍各类设备的功能、参数与使用方法，页面标题即为设备名称。
- `[Tutorial]`：提供系统性操作指南，适合引导读者逐步学习某项技能或流程。
- `[GUI]`：聚焦于软件相关的说明，如各项目设置、面板和操作界面的用法。
- `[Macro]`：介绍宏的创作和使用方式，也可发布由用户自身创作的宏。
- `[Bluemap]`：介绍蓝图的使用方式，也可发布由用户自身创作的蓝图。
- `[Code]`：介绍关于Code设备的用法，包括接口文档等，也可发布由用户自身创作的设备。
- `[Idioms]`：收录典型用法、最佳实践或约定俗成的工作流，帮助理解“惯例”。
- `[Sundries]`：用于归档无法归入其他标签的杂项内容，主题灵活但仍需简洁。

以下是面向Wiki编写者的开发频道：
- `[Diction]`：规范术语使用，写作的约定与表达建议。
- `[Discussion]`：用于记录规范拟定过程、设计思路和社区协商内容。建议使用子标签，以明确该页面的讨论主题，例如`[Discussion][Device] Some Disscussion About Perlin Noise`。
- `[Announcement]`：发布与 Wiki 相关的重要通知，包括当前计划与官方声明等，发布内容应当是社区讨论后达成共识的意见。

## 合法页面标题的巴科斯范式语法

```
<PageName> ::= <Tags> <TitlePart> <Space> "~" <Space> <EnTitle>
             | <Tags> <Space> <EnTitle>

<Tags> ::= <Tag> | <Tag> <Tags>

<Tag> ::= "【" <TagName> "】"

<TagName> ::= <CapitalLetter> <WordChars>

<TitlePart> ::= <Title>

<Title> ::= <WordChars> | <WordChars> <Space> <Title>

<EnTitle> ::= <EnWord> | <EnWord> <Space> <EnTitle>

<WordChars> ::= <LetterOrDigit> | <LetterOrDigit> <WordChars>

<EnWord> ::= <EnLetter> | <EnLetter> <EnWord>

<LetterOrDigit> ::= <ChineseChar> | <Digit> | <Letter>

<Letter> ::= <CapitalLetter> | <LowerLetter>

<CapitalLetter> ::= "A" | "B" | ... | "Z"

<LowerLetter> ::= "a" | "b" | ... | "z"

<EnLetter> ::= <CapitalLetter> | <LowerLetter>

<Digit> ::= "0" | "1" | ... | "9"

<ChineseChar> ::= (任意中文字符)

<Space> ::= " "

```