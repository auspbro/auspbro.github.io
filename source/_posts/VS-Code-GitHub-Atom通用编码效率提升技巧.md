---
title: VS Code & GitHub Atom通用编码效率提升技巧
date: 2017-09-06 14:18:26
tags:
---

## 1. 两个共同的目标

以下的内容，假定你已经读完 Atom 官方教程 [Atom Flight Manual](http://flight-manual.atom.io/) 的第一章：

> Chapter 1: Getting Started

否则不要看下去了，回去先把那一章读完再说……

工程师们使用的编辑器，有两个最基本的设计目标：

> 尽量少敲键盘、少出错、多做事
> 一定要可扩展、可定制、可积累

<!-- more -->

Mac 上的编辑器，工程师们最喜欢用的曾经是 TextMate，后来是 Sublimetext 和 Atom…… 它们都是这样的编辑器。

新手在 Atom 里敲键盘的时候，可能会“惊讶”地发现，括号、引号等等，都是成对出现的…… 比如，你按一下单引号，‘，实际出现的是一对单引号，而光标则闪烁在两个引号之间： '|' …… 圆括号、方括号、尖括号、大括号，等等，也都是这样的 —— 这就是工程师们不断给自己提高效率的一种方式，他们会想尽一切办法让自己可以少敲键盘、少出错，当然，为的是做更多的事。

现在问题来了，按一下引号 ‘，出来的是两个引号，然后光标在中间，于是你就可以在两个引号之间输入了…… 引号之间的内容输入完毕之后，你还得按一下右方向键→，右手移动的距离很大，因为右方向键在最右下角…… 很烦人是吧？

在 Atom 编辑器中，用 ⇧⌘P 呼出 Command Palette，在里面输入 init script，由于这个输入框里使用的是 Fuzy matching, 你输入到 s 的时候，估计 Application: Open Your Init Script 已经排在第一位了，当它是第一位的时候，直接回车键 ⏎ 就可以了…… 在 init script 文件里拷贝粘贴以下代码：

```
# move cursor across the ending symbols...
EndingSymbolRegex = /\s*[)}>\]/'";:=-]/
atom.commands.add 'atom-text-editor', 'custom:jump-over-symbol': (event) ->
  editor = atom.workspace.getActiveTextEditor()
  cursorMoved = false
  for cursor in editor.getCursors()
    range = cursor.getCurrentWordBufferRange(wordRegex: EndingSymbolRegex)
    unless range.isEmpty()
      cursor.setBufferPosition(range.end)
      cursorMoved = true
  event.abortKeyBinding() unless cursorMoved
```

然后用 ⇧⌘P 呼出 Command Palette，在里面输入 keymap，你应该能看到 Application: Open Your keymap 已经排在第一位了，当它是第一位的时候，直接回车键 ⏎…… 在 Keymap 文件里拷贝粘贴以下代码：
```
"atom-text-editor:not([mini])":
  "enter": "custom:jump-over-symbol"
```

其中这个 enter，你可以换成你自己喜欢的、习惯的，比如 tab，或者 shift-enter。

现在，用 ⇧⌘P 呼出 Command Palette，在里面输入 reload，你应该能看到 Window: Reload 已经排在第一位了，当它是第一位的时候，直接回车键 ⏎ 就可以了…… 你也能看到这个命令其实有个快捷键： ⌃⌥⌘L，但 Command Palette 太好用了，以后你就知道了，记忆过多的快捷键实在是负担，虽然一些基础快捷键（比如命令行工具里的一系列与 control 键 ⌃ 组合的快捷键）必须通过大量使用变成脊髓记忆……

Window: Reload 完成之后，你刚刚设定的快捷键就生效了。

> 整个 Atom 程序就是一个大的 HTML 文件，你刚刚的 Reload，就好像是在浏览器里刷新了一下一样。也由于整>个 Atom 就是个 HTML 文件，所以，理论上来讲，它里面的任何一个地方都是可以随心所欲地定制的 —— 只要你懂 HTML/CSS/JAVASCRIPT…… 虽然你现在搞不懂之前拷贝粘贴的代码，早晚你会懂的，放心吧。

现在你再按顺序依次敲以下键盘序列试试……

> ( ‘ t h i s ⏎ ⏎

最终的显示应该是这样的（其中最后的 | 表示的是闪动的光标）：

> (‘this’)|

你看，Atom 和所有现代文本编辑器一样，就是一个可高度定制的编辑器，它在 Github 上的标题就是这么写的：

> atom: The hackable text editor

处处都可以定制…… 可是，虽然这当然是好事儿，但，警告要来了：

> 只关注最重要的定制，不要在无关紧要的细节上浪费生命。

最重要的定制，是接下来要讲的 Snippets，无关紧要的细节指的是皮肤啊、配色啊之类的东西 —— 这种无关紧要的东西，快速做出选择，而后就再也不碰了，比如，Theme 我直个人就接选了一个 spacegray-dark-neue-ui，而 Syntax Highlight Color，我也选了同门的 spacegray-dark-neue-syntax，就完事儿了，在这上面多一秒时间都不要浪费……

## 2. Atom Snippets

先创建一个尾缀为 .html 的新文件，比如名称为 test.html，随便保存在那里，比如保存在桌面上（以便一会儿想删除的时候可以很快找到哈）…… 尾缀很重要，不要搞错。

然后在这个文件里按顺序敲以下键盘，其中最后一个 ⇥ 是神奇的 TAB 键：

> html⇥

在按 ⇥ 之前，你可以看到一个列表，先别管它，直接按 ⇥ …… 你会看到编辑器里突然多了一大堆东西：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>|</title>
  </head>
  <body>
  </body>
</html>
```

随便再输入点什么，比如，test html title，而后再按一下 ⇥，光标就挪到 <body> 之后、</body> 之前了。

能这么做，归功于 Atom 内建了 Html Language 的插件，其中设定了所有 HTML5 的标签“自动展开代码”（即，Snippets），该插件的项目代码托管在 [Github](https://github.com/atom/language-html) 上：

上面提到的这个 Snippets，代码如下：
```
'HTML':
    'prefix': 'html'
    'body': '<!DOCTYPE html>\n<html>\n\t<head>\n\t\t<meta charset="utf-8">\n\t\t<title>$1</title>\n\t</head>\n\t<body>\n\t\t$2\n\t</body>\n</html>'
```

你可以大概理解一下，其中：

> \n 表示的是纯文本中默认并不显示的“换行符号”；
> \t 表示的是 tab（不是 tab 键，而是文本中显示的“缩进” —— 通常相当于两个空格，或者四个空格）；
> $1 是展开后光标所在的位置；
> $2 是再次按 ⇥ 键的时候，光标应该所在的下一个位置……
> 你看，工程师们想了多少办法让自己少敲多少键盘…… 他们做事、干活，累的不是手啊，用的是脑！有脑子不用就等于没有脑子…… 不是吗？

在 Atom 编辑器中，用 ⇧⌘P 呼出 Command Palette，在里面输入 Open Your Snippets，你会看到里面有这样一段被注释掉（即，不被执行）的代码：
```
# Your snippets
#
# Atom snippets allow you to enter a simple prefix in the editor and hit tab to
# expand the prefix into a larger code block with templated values.
#
# You can create a new snippet in this file by typing "snip" and then hitting
# tab.
#
# An example CoffeeScript snippet to expand log to console.log:
#
# '.source.coffee':
#   'Console log':
#     'prefix': 'log'
#     'body': 'console.log $1'
#
# Each scope (e.g. '.source.coffee' above) can only be declared once.
#
# This file uses CoffeeScript Object Notation (CSON).
# If you are unfamiliar with CSON, you can read more about it in the
# Atom Flight Manual:
# https://atom.io/docs/latest/using-atom-basic-customization#cson
```

以后你就可以把自己写的 Snippets 放在这里（当然以后还有更高级的方法）……

在 Comman Palette 里，输入 Available，可以找到 Snippets: Available，不妨逐个玩一玩…… 给自己定个 Timer，玩上半小时之后一定要停下来。

现在可以回去再仔细看看 Atom 官方教程 [Atom Flight Manual](http://flight-manual.atom.io/) 的第二章了，尤其是 Snippets 那一节。

读完之后再回来。

Atom 的 Snippets 有个概念，叫 Scope，也就是说，根据文件类型确定究竟有哪些“自动展开”可以使用的。所以，你若是在一个 .md 的文件里敲击 html⇥，就不可能被展开成上面那个例子那样。

写 Snippets 的时候，如何确定 Scope 呢？请认真阅读这段话：
> If it’s difficult to determine the package handling the file type in question (for example, for .md-documents), you can also proceed as following. Put your cursor in a file in which you want the snippet to be available, open the Command Palette (cmd+shift+p), and run the Editor: Log Cursor Scope command. This will trigger a notification which will contain a list of scopes. The first scope that’s listed is the scope for that language. Here are some examples: source.coffee, text.plain, text.html.basic.
>
>摘自于：https://atom.io/packages/snippets

你现在看不懂无所谓，大致留个印象就好，反正以后都是通过反复用，反复研究产生真正理解的。

这里有个重要的建议：

尽量不要直接使用别人写好的 Snippets，要自己写、自己敲；而别人写好的，尽量只用来参考。

无论你学什么语言（或者框架），就一定要在学习的过程中不断添加积累自己关于那个语言（或者框架）的 Snippets，这是一个自己给自己“添麻烦”的过程，但这种所谓的“麻烦”，其实是一种刻意训练，不做其实是偷懒，偷懒这东西，真心不划算的。

我在参考别人写好的 Snippets 的时候，有个习惯，把那些他们定义的“缩写”全部换成自然语言。比如，人家原来写的是这样的：
```
"if/else statement":
  prefix: "ies"
  body: """
  if (${1:condition}) {
  \t${2://statements here};
  } else {
  \t${3://statements here};
  };${4}
  """
```
我会改成这样（注意 prefix 那一行）：
```
"if/else statement":
  prefix: "if/else statement"
  body: """
  if (${1:condition}) {
  \t${2://statements here};
  } else {
  \t${3://statements here};
  };${4}
  """
```

这是因为我觉得花时间记忆那么多缩写没必要，尽管他们编制的缩写也有一定的规律…… 因为在 Atom 编辑器里，无所不在的有一个叫 Fuzy Matching 的东西（也是工程师们设计好用来提升自己效率的东西）—— 它会不分字母前后顺序地帮你寻找你可能要输入的内容…… 在 Fuzy Matching 无所不在的情况下，缩写的意义其实并不大。

哪怕是把别人写好的 Snippets 如此修改一下的过程中（其实本质上好像完全没干什么），也有机会读一遍，也有机会留个印象，甚至因为在修改中可能出现错误而不得不反复读了很多遍 —— 那更好，因为那就加深了印象……

学会了 Snippets 的使用与编写，感觉上已经很工程师了，哈！早晚有一天你会为 Atom 写 Package 的 —— 到时候你就觉得没多难了。

### 参考文献：
* [Atom 编辑器进阶](http://lixiaolai.com/2016/06/17/makecs-atom-advanced/)
* [VS Code Tips and Tricks](https://github.com/Microsoft/vscode-tips-and-tricks)