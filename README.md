# 开发日志

## 为什么需要多进程架构？

**OpenArctic** 是一个“多进程架构”解决方案。我并不知道其他的开发者是否都了解多进程架构，但很明显这不是一个新概念。在服务端开发中，高性能的Web服务器（类似`apache2`和`nginx`）几乎都采用了多进程架构，它们称之为`worker`。而服务器集群（由各种不同角色的节点构成的集群）本身就是多进程架构。  
在最近的俩年中，我工作在客户端，有两个特别的系统让我很感兴趣： **Visual Studio Code** 和 **Chrome** 。

### Visual Studio Code

Visual Studio Code 有一个概念叫做[Language Server](https://code.visualstudio.com/api/language-extensions/language-server-extension-guide)。事实上，开发者需要使用JavaScript开发一个Visual Studio Code的Extension作为客户端。并且使用任意一种编程语言开发一个独立服务进程作为服务端。借此，Visual Studio Code能够有效的和不同的编程语言编写的模块进行交互。这是一种很棒的设计，让多种编程语言编写的模块运行在独立的进程中，却又能够协同工作。

### Chrome浏览器 (基于Chromium开源项目)

Chromium 项目同样基于多进程架构，并且对其进行了详细的[说明](https://www.chromium.org/developers/design-documents/multi-process-architecture)。值得一提的是，从视觉上书签栏以上的区域是由Chromium的主进程负责绘制的，而每一个网页由一个单独的Renderer进程负责绘制（事实比这要更复杂一些）。我相信大多数普通用户并不能意识到这一点，这说明以多进程的方式来构建图形界面是可行的。

### 一个中间件

我从上面两款应用中得到了一些启发，对于客户端开发来说，多进程架构比我想象的更有价值。但是，似乎并没有太多人关心这一点。在服务端开发中，有一个非常关键的中间件，Message Queue。当你试图搭建一个复杂的服务端系统时，一定涉及到多个不同角色的节点（例如Web Server、Database、Master、Slave等）。而要想这些节点在网络层面上能够交互，必然会用到消息队列。而经过一段时间的调研后，我发现在客户端开发领域并没有一个等价物。如果你想要实现类似于Visual Studio Code或者Chrome的模式，你需要付出非常大的努力。

因此，我开始尝试构建一个。

