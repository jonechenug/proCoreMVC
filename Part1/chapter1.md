回目录

# 第一章 ASP.NET Core MVC 的前世今生

ASP.NET Core MVC 是一个微软公司开发的Web应用程序开发框架，它结合了MVC架构的高效性和简洁性，敏捷开发的思想和技术和.NET 平台的最好的部分。在本章，我们将学习为什么微软创建ASP.NET Core MVC, 看看他和他的前辈的比较以及和其他类似框架的比较，最后，大概讲一下ASP.NET Core MVC里面有什么新东西，还有本书中包括哪些内容。
了解ASP.NET Core MVC的历史

最初的ASP.NET 诞生在2002年，那个时候，微软想要保持传统的桌面应用开发的霸主地位，又看出了因特网是一个潜在的威胁。下图展示了当时的微软的技术栈。

![Asp.net web forms 技术栈](/imgs/fig.1-1.png)


图1.1 ASP.NET Web Forms 技术栈


## ASP.NET Web Forms

微软当时试图使用Web Forms技术来隐藏超文本传输协议（HTTP），和与生俱来的无状态性；而且在当时，HTML 对许多程序原来说时陌生的，所以WebForms 使用用户控件在服务器端进行UI建模。 每个控件在各个请求之间跟踪着自己的状态，在需要的时候渲染成HTML，还有自动连接客户端事件（例如按钮的单击事件）以在服务器端执行响应代码。实际上，Web Forms 是一个巨大的抽象层，用来在Web上实现事件驱动的图形用户界面。

这个想法是基于想让Web开发的感觉就像开发桌面应用程序一样的理念。开发人员可以用有状态UI来思考并且不需要一系列无关的HTTP请求和响应。 微软可以无痛地将Windows桌面开发大军转向新的Web开发的世界。

## ASP.NET Web Forms 出了什么问题？

传统的ASP.NET Web Forms 开发理论上来说是很好的，但是实际应用上看非常复杂。

#### 
* 视图状态(View State) 太重: 在各个请求之间维护状态的机制导致了大块的数据在服务器端和客户端来回传递。 在一般的Web应用里，这些数据可能会达到数百KB。并且还要在每一个请求里跑一个来回，导致响应变慢，又增加了服务器的带宽占用。

* 页面的生命周期：将客户端事件接到服务器端，事件处理器代码的机制，是页面生命周期的一部分，但是它很复杂，并且很脆弱，基本上没有程序员能够在运行时操作控制器的层级关系而不产生视图状态错误。有一些事件处理程序会莫名其妙的失败。

* 错误的关注分离： ASP.NET Web Forms 后端代码模型，提供了一种将应用程序代码从HTML标记里分离成单独的类那文件里机制，就可以分离逻辑和表现层，但实际上，开发人员更愿意使用表现层代码和逻辑代码混合的方式。这最终导致应用程序的脆弱和不智能。

* 对HTML的有限的控制： 服务器端控件，将他们自己渲染成HTML，但是有一些HTML不是你想要的，在ASP.Net早期版本中不满足Web标准，或者不能很好地使用样式表(CSS)。服务器控件也生成了带有复杂ID的HTML标记，这样的ID使得Javascript程序难以访问。 这些问题虽然在最近的Web Forms版本中有所改善，但是也需要你是HTML编程专家才能使用它。

* 漏洞百出的抽象。 Web Forms 想要尽可能地隐藏HTML 和HTTP。 当你试图实现自定义的行为时，你经常掉进了抽象的泥坑里。强迫你逆向回发事件机制或执行迟钝的方案才能使他产生想要的HTML。

* 很差的测试性。 Web Form的设计者们当初没有意识到自动测试会变成软件开发中至关重要的组成部分。 他们设计的紧耦合的架构不适合做单元测试。集成测试也不容易。

Web Forms 并非都不好。微软在标准兼容性方面和简化开发流程方面做了很多努力，甚至从原始ASP.NET MVC 框架上提取了很多优秀的特性放到Web Forms里。当你需要快速看到结果时，就像有时候你需要让一个相当复杂的Web 应用程序在一个工作日内运行起来的话，使用Web Forms是相当合适的。但是除非你在开发的时候非常仔细，否则你会发现你所创建的应用程序会非常难以测试和维护。

## 原始的MVC 框架

2007年10月份，微软宣布了一个新的开发平台，构建于现有的asp.net 平台之上，算是对Web的各种批评和竞争对手如Ruby on Rails 直接的回应。新平台叫做ASP.NET MVC 框架。反映了Web应用开发的各种技术合并的趋势，例如HTML和CSS 标准化，RESTful Web服务，有效的单元测试，还有开发人员应该拥抱无状态(原文是有状态，应是错误)的HTTP的自然性质的想法。
现在看来，支持原始的MVC框架的概念是自然的且显而易见的，但是在2007年的时候，.NET web 开发的世界里是缺乏这些东西的。ASP.NET MVC 框架的推出使得微软的web开发平台重新走回了现代。
MVC框架也标志了微软态度的转变，它原本是想要控制Web应用程序开发工具链的所有东西。微软在MVC框架中采纳了新的理念，如基于开源工具JQuery, 接受设计约定和从竞争对手平台里学到的最佳实践，并对开发人员公开了MVC 框架的源代码。

## 原始的MVC 框架出现了什么问题?

在初创时期，微软基于已有的ASP.NET 平台上创建MVC框架是合理的，在其上面加了许多在开发过程中对初学者有用的帮助，如初始页等一些低级的功能，已经被ASP.NET开发人员所广泛认同和理解。
在Web Forms平台上嫁接出来的MVC 框架肯定做了一些妥协。MVC框架的开发人员变得习惯于使用配置文件和必须将代码做一些微调才能使应用程序得以运行。

随着MVC框架变得越来越流行，微软开始将一些核心特征加入到Web Forms中。结果就使 Web Forms 变得奇怪了。因为原来是用于支持MVC框架的一些设计原理用来扩展Web Forms，他们之间必须相互融合的很好。同时，微软开始扩展ASP.NET框架，以便创建Web Service和实时通信，每一个新的内容都加入了他们自己的配置和开发约定，每一个东西都有他自己的优点和奇异性，所以使得整个框架变得越来越支离破碎，也来越混乱。

## 理解ASP.NET Core

2015年，微软宣布了一个ASP.NET的新方向和一个新的MVC框架，最终产生了ASP.NET Core MVC, 本书的主要内容。

ASP.NET Core 是基于.NET Core建立的。 .NET Core 是一个.NET Framework的跨平台版本，没有专门针对于Windows的应用程序接口(API)的内容。Windows仍旧是一个占据支配地位的操作系统，但是Web 应用程序正在增加份额，一般是承载于云平台上的小的简单的容器里。由于转向了支持跨平台，微软扩展了.NET 的触角，使得ASP.NET Core应用在更广泛的环境中部署成为了可能。 作为一个奖励，也可以使得开发人员在Linux上和OSX/MacOS上能够创建ASP.NET Core web 应用程序。

ASP.NET Core 是一个全新的框架，他简单，易于上手，对来自Web Form的程序员来说还是很自由的。 而且因为他是基于.NET Core的，它支持在平台和容器中进行Web应用程序的开发。
ASP.NET Core MVC 提供了在新的ASP.NET Core平台上建立原始的ASP.NET MVC 框架的功能。 包括以前Web API提供的功能，包括了使用更自然的方式产生复合内容，也可以让关键的开发任务，如单元测试，变得更简单更具有可预见性。

## ASP.NET Core MVC 的主要优点

下列章节简要介绍了新的MVC平台怎样克服了原有的Web Forms和原始MVC框架带来的问题，将ASP.NET重新带回到技术前沿。

#### MVC 架构

ASP.NET Core MVC 遵循了model-view-controller(MVC) 模式，它规定了ASP.NET web 应用的结构形式和它包含的组件之间的交互方式。
区分MVC架构模式和ASP.NET core MVC实现是很重要的。 MVC模式不是新的，他可以往回追溯到1978年的Xerox PARC 的Smalltalk 项目。但是，他在web应用中的使用变得越来越广泛，有以下原因：
用户与应用程序的交互遵循自然的生命周期：用户使用一个action更改他的数据模型并将一个新的更新过的视图交还给用户。 这个循环一直重复下去。这对于web应用来说,处理一系列的HTTP请求和相应是非常方便的。

Web 应用使几种技术（数据库，HTML，可执行的代码等）结合在一起成为必然，这些技术通常被分成一系列的层。这些技术的结合的模式即映射成了MVC模式中的一些概念。
ASP.NET Core MVC 实现了MVC模式，并且相对于Web Form 应用来说，大大改进了分离程度。实际上，ASP.NET Core MVC 实现了许多MVC 模式，尤其是适合于Web 应用的那些。你将会在第三章学到更多的关于这个架构的理论和实践。



#### 扩展性
ASP.NET Core和ASP.NET Core MVC 是由一系列独立的具有良好定义特征的，符合.Net 接口要求的或者建立在抽象基类基础上的组件构成的。你可以很容易地用你自己定义的组件替换掉任何一个关键组件。一般来说，ASP.NET Core MVC 为每一个组件提供三个选择：

#### 使用默认的实现（通常可以满足大多数应用的要求）。
使用一个继承自默认实现的类来对组件的行为做轻微调整。
由满足接口规范的完全由你自定义的新的实现类来替换默认的组件。
从14章开始你将会学到各种不同的组件，并讲解怎样和为什么你想要做轻微的调整或完全替换他们。

#### 对HTML和HTTP更全面的控制
ASP.NET Core MVC 能够产生出干净的、与标准兼容的标记，它内建的标记帮助器能够产生与标准兼容的输出。但是与Web Forms 有着显著的逻辑上的不同。ASP.NET Core MVC 鼓励你构造简单的、优雅的、具有CSS修饰的标记，而不是产生出一堆你几乎不能控制的HTML代码。
当然，如果你想使用现成的UI组件来制作复杂的UI元素，例如日期选择器控件或层叠菜单，ASP.NET Core MVC无需特殊需求的方式可以让你很容易地使用那些最好的客户端库，例如jQuery，Angular，或Bootstrap CSS库。ASP.NET Core MVC 会让那些库相处的很好，甚至微软已经将他们作为内置的标准模板的一部分。

ASP.NET Core MVC 与HTTP是很和谐的，你可以控制在浏览器和服务器之间传递，所以，你可以尽可能使用你的经验来做细微调节。Ajax 可以变的很容易，建立一个web 服务来接受浏览器的HTTP请求是一个很简单的过程，就像第20章描述的那样。

#### 可测试性
ASP.NET Core MVC架构给你提供了优秀的可维护性和可测试性，因为它可以自然地将你所关心的部分分割成独立的一块块。另外，ASP.Net Core 平台的每一部分和ASP.NET Core MVC 框架可以因为测试的目的完全分离并替换，并可以使用任何主流的开源测试框架来进行测试，例如我们将在第七章介绍的xUnit。

在本书中你将看到如何为ASP.NET Core MVC 控制器和动作(Action)编写干净、简单的单元测试，我们将采用假的或虚构的框架组件实现来仿真任何应用场景，使用各种不同的测试和模拟的策略。即是你从来没有写过单元测试，你也将有一个好的开始。

可测试性不仅对单元测试来说是很重要的，ASP.NET Core MVC应用也具有良好的UI自动测试工具，你可以编写测试脚本来仿真用户输入，而不用去分析HTML元素的结构，CSS类，或框架生成的ID。你也无须担心程序结构在测试中无意地受到破坏。

#### 强大的路由系统
随着web应用程序技术的提高，统一的资源描述符(URLs)的风格也进化了。
例如像这样的URL：
/App_v2/User/Page.aspx?action=show%20prop&prop_id=82742
已经大幅度减少了，被替换成更简单的更干净的格式，像这样：
/to-rent/chicago/2303-silver-street
有几方面的原因让我们来考虑URL的结构。 首先，搜索引擎会给出在URL中出现关键字的权重。 例如，对”rent in Chicago” 的搜索会得到更多的更简单的网址。其次，许多web用户现在变得更有文化，已经能足够理解通过输入到浏览器的地址栏的URL并欣赏导航选项了。第三，一些人懂得URL的结构，他们将更愿意去连接它，给朋友分享它，甚至能在电话里通过大声读出的方式传给对方。第四，他不会向公共互联网曝露你的应用程序的技术细节，文件夹和文件名的结构。所以你可以自由地改变底层的实现，而不会破坏你的所有链接。 

早期的框架很难实现干净的URL，但ASP.NET Core MVC使用URL路由提供默认的干净URL。这使您可以控制您的URL模式和您的应用程序之间的关系，为您提供自由创建一个对用户有用的无需符合预定义模式的有意义的URL模式。当然这意味着，如果你愿意，可以轻松地自定义一个现代REST风格的URL模式。
第15章和第16章你将学到更多关于URL路由的内容。 

#### 现代的API
微软的.NET 平台随着每一次主版本的发行而进化，支持甚至引领着现代编程的当前技术水平。ASP.NET Core MVC 是为.NET core创造的，因此它的API 可以利用最新的语言和运行时特性，这些新东西已经广为程序员所熟悉，包括await关键字，扩展方法，Lambda表达式，匿名和动态类型，集成语言查询(LINQ)等。
许多ASP.NET Core MVC API 方法和代码模式遵循比早期版本更干净，更具表达力的原则。 但是如果你没有了解最新的C#语音特性，别担心，我们将在第四章提供一个最重要的C#语言特性的内容总结。

#### 跨平台
以前的ASP.NET版本是专门用于Windows平台的，必须使用Windows 桌面版来编写web应用，并且要使用windows server来发布并运行它。微软让ASP.NET Core 跨平台了，无论开发还是发布都可以跨平台。 .NET Core 可用于很多不同的平台，包括Linux, OS X/mac OS ，也可能将来兼容其他的平台。
大多数ASP.NET Core MVC开发可能都会使用Visual Studio 来开发，但是微软也创建了跨平台的开发工具，叫做Visual Studio Code, 这意味着ASP.NET Core MVC的开发不必局限于windows。

#### ASP.NET core MVC 是开源的
不像以前的微软web 开发平台，你现在可以自由地下载ASP.NET Core 和ASP.NET Core MVC的源代码，甚至可以修改并编译你自己的版本。当你调试进入系统组件，并且你想要单步跟踪进入源代码内部一看究竟，开源对你来说是非常重要的。另外，如果你想编写一个高级组件，要想看看开发的可能性是否存在，或者研究内建的组件内部是如何工作的，开源也是非常有用的。
你可以在这里下载asp.net core 和ASP.NET Core MVC 源码:
https://github.com/asp.net.

## 读这本书我们需要什么基础？
要想从本书里面学到更多东西，你应该熟悉web开发基础知识，了解HTML 和CSS是如何工作的，具有C#的编程经验。 如果你对客户端的知识例如javascript不甚了解， 没有关系，这本书的重点在服务器端开发。你可以从例子中选取你需要的内容。 在第四章，我们将会总结大多数有用的C# 语言特征，尤其对MVC的开发，你将会发现更新到最新的.NET版本是很有用的。

## 这本书的结构是怎样的？
这本书分成两部分，每一部分都包含相互之间有关联的主题。

#### 第一部分： 介绍ASP.NET Core MVC
我是从ASP.NET Core MVC开始介绍这本书的。我解释了MVC模式的好处和对实践的影响。讨论了ASP.NET Core MVC 适合现代web开发，并描述了开发工具和C# 语言特征，这些东西是每个ASP.NET Core MVC程序员都需要了解的。

第2章，我们将深入一个创建简单web应用的过程，从中获得主要的组件和构建块，以及它们之间如何相互匹配等相关知识。但是这一部分的主要内容，是通过一个叫做SportsStore项目，在该真实项目的从构思到发布完整的开发周期中，向你展示ASP.NET Core MVC的开发过程。

#### 第二部分 ASP.NET Core MVC详解
第二部分，我解释了用于构建SportsStore 的ASP.NET Core MVC 特性的内部工作原理。 向你展示了每一个功能是如何工作的，解释了他扮演的角色，展示了可用的配置和定制选项。在第一部分提出的广泛的内容，在第二部分都会给予更深入的探讨。

## 这一版有什么新东西？
本版已对ASP.NET core MVC的描述做了修订和扩展，它反映了微软支持Web开发的方式的完全转变。早期版本的MVC框架是建立在ASP.NET Web窗体的基础上的。这在为MVC开发提供了一些成熟的基础方面有一定的优势，但是这样做的方式泄露了web form的工作细节。一些功能暴露了Web form内部的方式，在MVC中没有任何这方面的负担，应用程序和其他功能不会产生不可预知的结果。        
此外，ASP.NET提供了.NET框架基础应用组件，这意味着只有在微软发布新版本时才可能进行重大更改。这是一个问题，因为网站发展的变化速度超过了.NET的变化。             ASP.NET core MVC是经过了完全重写的，保留了前版本的总体设计哲学，但使用了更新的API，以提高Web应用程序性能。ASP.NET core MVC 建立在ASP.NET core，它本身就是建立在一个完全的重写的网络协议栈的基础上。基本的Web form已经消失，与.NET Framework的紧密耦合已被打破。
如果你有MVC 5的经验，你会发现变化的程度是惊人的，但不要恐慌，基本概念是相同的，许多重大的变化其实没有那么复杂。在这本书的第2部分，我总结了每个主要特点的变化，以减轻从MVC 5 到 ASP.NET core MVC的过渡的痛苦。 

## 去哪里下载代码？
你可以从Apress.com上下载本书中所有章节的全部的例子。下载无需付费，包括所有的工程代码和内容。你不用非得下载代码，但是使用这些例子，将这些代码剪切并粘贴到你自己的项目中将会是最容易的实验方法。

## 小结
在本章，我们介绍了ASP.NET Core MVC 的前世今生，它从Web Forms 到原来的ASP.NET MVC 框架的进化过程。我们描述了使用ASP.NET Core MVC的好处、本书的结构，和你需要跟着学习的软件。 下一章，你将会看到ASP.NET Core MVC框架的实例，一个介绍那些好处的简单演示。