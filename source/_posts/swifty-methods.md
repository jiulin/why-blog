title: 用 Swift 来写 Swift 的方法
tags: Swifty
date: 2014-11-16 19:33:04
categories: 翻译笔记
description: 甩掉 Objective-C 的历史包袱，做一名纯粹的 Swift 程序员。
---


绝大部分学习 Swift 的程序员都有一定的 Objective-C 的开发经历。在过去的时光里，很多人使用 Objective-C 开发 iOS 和 Mac 应用。如果你正在阅读这篇文章，那么你很有可能就是其中一员。我们很容易把写 Objective-C 时候的那一套心得照搬到 Swift 开发里，很容易把一些现有的习惯和想法应用到 Swift 中。但是，和 Objective-C 这个前任相比， Swift 是一门完全不同的语言，我们应该以一种全新的心态去对待它，而不应该一直带着以前的历史包袱。

目前，有很多文章对 Swift 的新特性进行讲解，比如结构体、枚举类、元祖和泛型等等。有些人甚至尝试写一些[函数式编程](http://robots.thoughtbot.com/efficient-json-in-swift-with-functional-concepts-and-generics)的代码，但是今天我想谈一些更细致的内容：方法。

在 Objective-C 的论坛里，就算是方法命名这种琐碎的东西也有大量的讨论和争议。这门语言的特点之一便是命名冗长，比如 `performSelectorOnMainThread:withObject:waitUntilDone:` 或是 `tableView:targetIndexPathForMoveFromRowAtIndexPath:toProposedIndexPath:` 。对于这种命名习惯，有人喜欢，有人不喜欢，双方吵了很久，但是一直没有个结果。事实上，这个就是 Objective-C 的规范。规范是个好东西：在定下了通用的编码风格之后，协作成员可以更好地阅读彼此的代码，提高合作效率。

但是，对于 Swift 这门全新的语言来说，编码规范尚未形成，现在还是百家争鸣的阶段。那么问题又来了：撇开命名习惯不谈，就针对 Swift 这门语言来说，它有很多新的特性，可以让我们更加清晰地定义方法，而不用像 Objective-C 那样冗长。

接下来就让我们开始全新的旅程吧！


## 冗长 ≠ 清晰

对于冗长繁琐的 Objective-C 风格，一个常见的解释是这样可以让代码清晰易懂。他们说这样的代码可以自我解释，不用写文档大家就可以理解这个方法是用来干吗的。

好吧在一定程度上来讲，这是完全正确的。不可否认，很多方法从命名就可以知道它是用来干吗的。但是我常常发现很多 Objective-C 的方法是啰嗦而又冗长的，并不够清晰。实际上，大部分代码让本来几个词就能解释的东西变得复杂化，很多的方法并不用那么复杂。

举个例子：

    [string componentsSeparatedByString:@"\n"];

这命名看得我也是醉了，足足27个字母，但是这真的 **清 晰** 吗？我知道你肯定知道这方法是干吗使的，但是假设，你不知道，对，假设你是一个小白，看到这么一堆代码，你猜猜它是干吗用的？唔它肯定是用来分离某个东西的，它需要一个字符串参数然后把它拆成了数组嗯。然后你可能根据它的上下文猜出了正确答案。但是 `components` 是个什么鬼？可能是指一些藏在内部的不起眼的东西？如果我第一次看到这个方法，我能猜个八九不离十，但是在看到文档之前我始终无法下定论。


不妨和 Ruby 做个比较：

    string.split("\n")

它好短！很显然它的参数是一个字符串，然后好像做了什么分离。和上次一样，我们也只能猜个七七八八，瞄个大概95%的样子。和上面那个方法相比，这个方法我们也无法完全猜出其用意，但是单词量再多也是无济于事的。



## 我们有办法

前面说了那么多，我希望你能理解，不管命名多么的复杂，它都无法准确的表达它的用处。如果命名清晰，那很好，确实有助于理解，但是命名终究只是一个标记而已，无法代替文档的作用。

那么接下来我们假设这样一个场景，一名年轻的 Swift 程序媛正在认真学习，然后她在人生中第一次遇到 `map` 、 `filter` 和 `reduce` 这三个家伙。光看名字她能理解这三个方法的用法吗？显然不能。当你理解它们的用处的时候再去看命名，当然是十分清晰，但是现在假设我们是第一次遇到这些方法，我们显然有点一头雾水。

但是你可以这样做：按住 option 键，点击方法名：

![](http://callmewhy.qiniudn.com/swifty-methods-xcode-yosemite.png)


是的就是这么简单！猜什么猜，直接按住 option
 点它就行！点完之后你就可以看到方法的解释和参数的含义了，等你理解了它的用处你就会发现， `map` 的用处和 `arrayByEnumeratingObjectsUsingBlock` 这一大串的功能是一样的。简单粗暴，直接有效。


## “噪音”

有人说，语意清晰比语句简洁更重要。没错，他们说的很对。但是简洁不代表不清晰。中国有个成语，言简意赅，就是说的这个。复杂冗长的命名同时也带来了很多负面影响。虽然现有的工具可以自动补全，这样写起来似乎也挺轻松的。但是在阅读的时候，啰嗦繁琐的命名总归是要消耗开发者大量的时间去阅读和理解，而言简意赅的命名读起来要轻松很多。


我们再来看个简单例子，声明一个日期对象：

    NSDate *date = [NSDate date];

为什么我们要说这么多遍的 date ？既然从上下文来看这个 date 很显然是个日期对象，为何还要啰嗦的再说一遍？“我用日期类定义一个日期对象并用日期类的日期初始化”，这可真是磨人。而且要分号干什么？

和 Swift 比较一下：

    let date = NSDate()

显然这明显要简单得多。在这场对比里， Swift 赢了。双方的代码都很清晰的告诉我们它们要干什么，但是后者的“噪音”更少，只暴露出了最关键的内容。这样我们需要理解的代码少了很多，阅读起来也轻松不少。


再来看下另一个例子：

    [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(foo:) userInfo:nil repeats:NO]



请盯着 `scheduledTimerWithTimeInterval` 这个方法，凝视一分钟。为什么要再说一遍 `timer` ？都已经说了，是 `NSTimer` 的方法，我们还能 schedule 别的东西不成？而且 `interval` 已经足够清晰了，为什么还要多此一举的用 `time interval` ？

再看看 Swift ：

    NSTimer.schedule(interval: 1.0, ...)

注意我把参数里的 `with` 移除了，然后用了 `interval` 作为参数标记。 Objective-C 总是想把方法名搞成个句子，但是现实是编程追求的是简洁和清晰，不可能像现实生活那样说话。 (如果你继续抱有这个想法，恭喜你，你和 AppleScript 想法一致，当然，没人喜欢 AppleScript )


不妨引用一下 Swift 的设计者 [Chris Lattner](https://devforums.apple.com/message/1023082#1023082) 的一段话：

> 可选链中的问号 (?) 的目的是让代码更清晰。很多人其实误解了 Swift ：它的目的，不是缩短语句减少单词量，而是为了移除多于的东西，保留最核心的内容，从而让代码尽量清晰明确。

这段话看你怎么理解了，但是我是这么解读的：我并不是为了简洁而简洁，我是通过移除“噪音”达到言简意赅的目的。


## 此处有争议

编程语言各有各的特色，各有各的习惯，切忌把命名习惯应用到所有的语言里。有些方法是广为流传的，当然也有一些属于冷门招数，鲜为人知。

想一想这些函数和方法的命名：`map` 、 `reduce` 、 `stride` 、 `splice` 。他们都是简短且易于使用的。可能第一眼看到不太能理解，但是这问题不大，因为它们都是标准类库的方法，你会用到很多很多次，抬头不见低头见，所以虽然第一次接触的时候需要一些学习的成本，但是从长远来看是利大于弊的。

但是从另一个角度来说，你也会有很多你自己应用的代码，你显然不想把你代码里的方法定义成这种单个单词的名字。毕竟在你的整个代码库中，它们可能只会被调用一两次。它们是为了完成特定的任务而存在的，当你用到其他类的时候，你就完全忘了这些代码。

我希望你能理解我说的内容。如果你在写一个类库或者框架的代码，或者是一些要被大范围使用的基类，那么可以用一些简洁的命名。如果你是在写应用里的普通代码，只会在一定范围里用到的而且使用频率并不频繁，那么你需要更加清晰更加[具有描述性](https://signalvnoise.com/posts/3531-intention-revealing-methods)的代码。


## 到底加不加标签

我觉得方法内的参数标签是个好东西 - 在函数调用的时候你可以直接理解参数的含义，而不是根据参数的位置去猜测。

你可能会据理力争，“从上下文来推测的话，不用参数标签我就能看懂参数的意思啊，而且写起来的时候我们有自动补全也很方便”。或许是这样的，不过不像我前面说的那些内容，参数标签是 Swift 的语言风格，因为这门语言本身就希望你去用它们，并且如果不用的话需要做很多额外的工作。

不过我认为有些场景确实不用标签更合理，比如下面这个：

    CGPoint(x: 10, y: 10)
    CGRect(x: 5, y: 5, width: 100, height: 50)


我觉得在这里，标签有点多余了，甚至有点影响视线。我们整天和坐标打交道，参数的顺序从来就没变过， x 总是在 y 前面， width 总是在 height 前面，如果不用标签的话其实也不错：

    CGPoint(10, 10)
    CGRect(5, 5, 100, 50)

很显然我们都明白这些参数是什么意思，加上标签反而把事情搞复杂了。

另一方面，有些情况下即使标签本身没有含义，但是加上之后会变得更加易于理解：

以 stride 为例：

    stride(from: 1, through: 100, by: 2)

如果没有这些标签，我们很难在没有文档的前提下搞清楚这些参数的含义。


## 可选参数

下面是在 Mac 下新建一个 window 对象的方法：

    [[NSWindow alloc] initWithContentRect:frame styleMask:NSTitledWindowMask backing:NSBackingStoreBuffered defer:NO screen:nil]

那么问题来了：在这一串方法中，唯一有意义的东西是内容的 frame 。其他的所有东西都改靠边站：他们太吵了！

Swift 很好地解决了这个问题，你可以设置参数的默认值，所以你可以这样定义初始化方法：

    init(contentRect: NSRect, styleMask: NSWindowMask = NSWindowMask.Titled, backing: NSBackingStoreType = .Buffered, defer: Bool = false, screen: NSScreen? = nil)

于是我们可以这样简短的初始化：

    NSWindow(contentRect: frame)

如果你需要添加其他参数，和往常一样加在里面即可。

Swift 中有个挺赞的特性，如果你有一串可选的参数，而你想设置第四个参数是没有默认值的，你不需要把前三个参数都写出来，有默认值的参数在 Swift 中是顺序无关的。

## 总结

啰嗦了这么多，你并不一定完全赞同我的观点，这也不是我的目的。我希望的是，你在写 Swift 的时候可以保持一个开放的心态，尽情探索这门有趣的语言，能够和新特性与新风格一起愉快的玩耍，享受编程的乐趣。如果你有时间，可以看看 Ruby 和 Haskell ，看看它们是怎么工作的。千万别听什么专家的话，因为这个领域根本就没有专家。当然，我也不是，括弧笑。


尝试走一些非主流的路线。记住，没有什么习惯，没有什么官方编程风格。我亲爱的读者朋友，你就是那个定义规则的人，在这门语言里，你就是专家，你就是主人。走自己的路，让别人说去吧。


## 关于 Swift 的资源

- [Great Swift resources](http://radex.io/swift/resources/)



***

原文地址：

- [Swifty methods](http://radex.io/swift/methods/)