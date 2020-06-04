#Window对象#
***

##定时器##
setTimeout()和setInterval()可以用来注册在指定时间之后单次或者重复调用的函数。它们都是客户端JavaScript中重要的全局函数，所以定义为Window对象的方法，但是作为通用函数并不会对窗口做什么事情。

* setTimeout()方法用来实现一个函数在指定的毫秒数之后运行。setTimeout()返回一个值，这个值可以传递给clearTimeout()用于取消这个函数的执行。
* setInterval()方法通用会在指定毫秒数的间隔里重复调用，和setTimeout()一样，有一个返回值可以传递给clearInterval()，用于取消后续函数的调用。

如果以0毫秒来调用setTimeout()，那么指定的函数不会立即执行。会把它放到队列里，等到前面处于等待的事件处理程序全部执行完成后，再'立即'调用它。

## 浏览器定位和导航 ##

Window对象的location属性引用的是Location对象，它表示该窗口中当前显示的文档的URL，并定义了方法来使窗口载入新的文档。
Document对象的location属性也引用到Location对象
>window.location===document.location  //总是返回true

Document对象也有一个URL属性，是文档首次载入后保存该文档的URL的静态字符串，而如果定位到文档中的片段标识符，Location对象会做出相应的更新，但是document.URL属性却不会改变。

### 解析URL ###

location对象的href属性是一个字符串，后者包含URL的完整文本。Location对象的toString()方法返回href属性的值，因此在会隐式调用toString()的情况下，可以使用location替代location.href。

这个对象的其它属性：protocol、host、hostname、port、pathname和search，分别表示URL的各个部分。称为'URL分解'属性

### 载入新的文档 ###

Location对象的assign()方法可以是窗口载入并显示你指定的URL的文档。replace()也类似，但它在载入新文档之前会从浏览历史中把当前文档删除。如果脚本无条件载入一个新文档，replace()可能比assign()是更好的选择。否则'后退'按钮会把浏览器带回到原始文档，而相同的脚本则会再次载入新文档。

此外Location对象还定义了reload()方法，可以让浏览器重新载入当前文档。
>使浏览器跳转到新页面的一种更传统的方法是直接把新的URL赋给location属性。
>同样可以把相对URL赋给location属性，location对象的URL分解属性是可写，对它们重新赋值可以改变URL的位置，并导致浏览器载入一个新的文档。

### 浏览历史 ###

Window对象的history属性引用的是该窗口的History对象。History对象是用来把窗口的浏览历史用文档和文档状态列表的形式表示。Historydioxide的length属性表示浏览历史列表中的元素数量，但出于安全考虑，脚本不能访问已保存的URL。

History对象的back()和forward()方法与浏览器的'后退'、'前进'按钮一样：它们是浏览器在浏览历史中前后跳转一格。第三个方法go()接受一个整数参数，可以在列表中向前（正参数）向后（负参数）跳过任意多个页。

## 浏览器和屏幕信息 ##

#### Navigation对象 ####
Window对象的navigation属性引用的是包含浏览器厂商和版本信息的Navigation对象。navigation对象有4个属性用于提供关于运行中的浏览器版本信息，并且可以使用这些属性进行浏览器嗅探。

* appName   web浏览器的全称。
* AppVersion   包含浏览器厂商和版本信息的详细字符串。
* userAgent  浏览器在它的USER-AGENT HTTP头部中发送的字符串，通常包含appVersion中的所有信息，并且常常也可能包含其他细节。
* platform 在其上运行浏览器的操作系统的字符串

### Screen对象 ###

Window对象的screen属性引用的是Screen对象。它提供有关窗口显示的大小和可用的颜色数量的信息。属性width和height指定的是以像素为单位的窗口大小。属性availWidth和availHeight指定的是实际可用的显示大小，它们排除了像桌面任务栏这样的特性所占用的空间。属性colorDepth指定的是显示的BPP值。

Window.screen属性和引用的Screen对象都是非标准但广泛实现的。可以用Screen对象来确定wed应用是否允许在一个小屏幕的设备上。如果屏幕空间有限，可能要选择用更小的字体和图片等等。

## 对话框 ##

Window对象提供了3个方法来向用户显示简单的对话框。

* alert()向用户显示一条消息并等待用户关闭对话框
* confirm()也是显示一条消息，要求用户单击'确定'或'取消'按钮，并返回一个布尔值
* prompt()同样显示一条消息，等待用户输入字符串，并返回那个字符串。

这些对话框会破坏浏览体验，如今对这些最常用的的应用就是调试。confirm()和prompt()方法会造成阻塞，在用户关掉它们显示的对话框之前，它们不会返回。意味着弹出对话框，代码就会停止运行。

还有更复杂的showModalDialog()，显示一个包含HTML格式的'模态对话框'。可以给它传入参数，以及从对话框里返回值。

## 错误处理 ##

Window对象的onerror属性是一个事件处理程序，当未捕获的异常传播到调用栈上时就会调用它，并把错误消息输出到浏览器的JavaScript控制台上。如果给这个属性赋一个函数，那么只要这个窗口中发生了JavaScript错误，就会调用改函数，即它成了窗口错误处理程序。

## 作为Window对象属性的文档元素 ##

如果在HTML文档中用id属性来为元素命名，并且如果Window对象没有此名字的属性，Window对象会赋予一个属性，它的名字是id属性的值，而它们的值指向表示文档元素的HTMLElement对象。

在客户端JavaScript中，Window对象是以全局对象的形式存在于作用域链的最上层，这就意味着在HTML文档中使用的id属性会成为可以被脚本访问的全局变量。

但是如果window对象已经具有此名称的属性，就不会发生。


