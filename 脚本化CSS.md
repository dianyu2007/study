# 脚本化CSS #
***

## CSS概述 ##

HTML文档的视觉包含很多变量：字体、颜色、间距等。CSS标准列举了这些变量，我们成为样式属性。CSS定义了这些属性以指定字体、颜色、外边距、边框、背景图片、文本对齐方式、元素尺寸和元素位置。

## 重要CSS属性 ##

* position 
* top、left
* bottom、right
* width、height
* z-index
* display
* visibility
* clip
* overflow
* margin、border、padding
* background
* opacity

### CSS定位元素 ###
css的position属性指定了应用到元素上的定位类型：

* static 默认属性
* absolute 该值指定元素是相对于它包含的元素进行定位。
* fixed 指定元素相对于浏览器窗口进行定位。
* relative 当设置为relative，元素按照常规的文档流进行布局，定位相对于它文档流中的位置进行调整。

当元素的position设置了除static之外的值，就可以通过元素left、top、right和bottom属性的一些组合指定元素的位置。

### 元素显示和可见性 ###

两个css属性影响了文档元素的可见性：visibility和display。visibility属性当其设置为hidden时，改元素不显示，当其值设置为visible时，改元素显示。display更加通用，它用来为接收它的容器指定元素的显示类型。它指定元素是否是块状元素、内联元素、列表项等。但是display设置为none时，受影响的元素将不显示，甚至不会有布局。

visibility和display属性之间的差别可以从它们对静态或相对定位的元素中看到。对于一个常规布局流中的的元素，设置visibility属性为hidden使得元素不可见，但是在文档布局中仍保留了它的空间。类似的元素可以重复隐藏和显示而不改变文档布局。但是，如果元素的display属性设置为none，在文档不居中不再给它分配空间，它各边的元素会合拢。在创建展开和折叠轮廓的效果时display属性很有用。

### 颜色、透明度和半透明 ###

可以通过css的color属性指定文档元素包含的文本颜色，并可以用background-color属性指定任何元素的背景颜色。

css允许指定元素确切的位置，尺寸，背景颜色和边框颜色，因为能绘制矩形和水平、垂直线条它有了基本的图形能力。

如果没有为元素指定背景颜色或图像，它的背景通常透明。指定的元素为半透明也是可以能的，css3中opacity属性来处理，该属性值为0~1之间的数字。

### 部分可见：overflow和clip ###

visibility属性可以让文档元素完全隐藏，而overflow和clip属性允许只显示元素的一部分。overflow属性指定内容超出元素的大小时该如何显示。允许的值：

* visible 默认值 内容可以溢出并绘制在元素的边框的外面
* hidden 裁剪掉和隐藏溢出的内容
* scroll 元素一直显示水平和垂直滚动条。
* atuo 滚动条只在内容超出元素尺寸时显示，而非一直显示。

clip属性确切的指定了应该显示元素的哪部分，它不管元素是否溢出。在创建元素渐进显示的脚本效果时候该属性特别有用。clip属性的值指定了元素的裁剪区域。

## 脚本化内联样式 ##

脚本化css最直接的方法就是更改单独的文档元素的style属性。类似大多数的HTML属性，style也是元素对象的属性，它可以在JavaScript中操作。但是style属性不同寻常，它的值不是字符串，而是一个CSSStyleDeclaration对象。该style对象的JavaScript属性代表了HTML代码通过style指定的CSS属性。

使用CSSStyleDeclaration对象的style属性时，所有的值都应该是字符串。同样所有的定位属性也是字符串且都需要包含单位。

如果通过计算的值来设置定位属性，需要保证在最后增加单位。

### CSS动画 ###

脚本化的CSS最常见的用途之一是产生视觉动画效果。使用setInterval()或setTimeout()重复调用函数来修改元素的内联样式达到目的。

CSS3的过渡模块定义了样式在指定动画效果的方式，实现过渡效果，从而避免使用任何脚本。

## 查询计算出的样式 ##

元素属性的style属性代表了元素的内联样式，它覆盖所有样式表，它是设置CSS属性值来改变元素的视觉表现最好的地方。但是在查询元素实际应用的样式时用处不大。因此想要使用计算样式，元素的计算样式是一组属性值，它由浏览器通过内联样式结合所有的链接样式表中所有可应用的样式规则后导出得到的。类似内联样式，计算样式也是用一个CSSStyleDeclaration对象来表示的，区别是计算样式是只读的。虽然不能设置这些样式，但是元素计算出的CSSStyleDeclaration对象确切地决定了浏览器在渲染元素时使用的样式属性值。

用浏览器窗口对象的getComputedStyle()方法来获得一个元素的计算样式。

getComputedStyle()方法返回值是一个CSSStyleDeclaration对象，它代表了应用在指定元素上的所有样式。

* 计算样式的属性是只读的。
* 计算样式的值是绝对值：类似百分比和点之类的相对的单位将全部转换为绝对值。
* 不计算复合属性，它们只基于最基础的属性。（不要查询margin值，使用marginLeft）
* 计算样式的cssText属性未定义

计算样式属性也有欺骗性，查询它们得到的信息不总是如人所愿。例如查询font-family属性,为适应跨平台可移植性，它可以接受以逗号隔开的字体系列列表。查询font-family属性时，可能返回类似字体系列列表，无法确切实际使用了哪种字体。

## 脚本化CSS类 ##

通过内联Style属性脚本化CSS的一个可选方案是脚本化HTML的class属性值，改变元素的class就改变了应用于元素的一组样式表选选择器，它能在同一时刻改变多个CSS属性。

HTML元素可以有多个CSS类名，class属性保存了一个用空格隔开的类名列表。className属性只能指定零个或一个类名，如果有多个类名就无法工作了。

HTML5解决了这个问题，为每个元素定义了classList属性。该属性是DOMTokenList对象：一个制度的类数组对象，它包含元素的单独类名。add()和remove()从元素的class属性中添加和清除一个类名。toggle()表示如果不纯在类名就添加一个，否则就删除它。最后contains()方法检测class属性中是否包含一个指定的类名。

类似其它DOM集合类型，DOMTokenList对象实时地代表了元素类名集合，而且在查询classList属性时类名的一个静态快照。如果从元素的classList属性中获得了一个DOMTokenList对象，然后这个元素的className属性改变了，这些变化在标识列表中及时可见。同样改变标识列表，在className属性中及时可见。

## 脚本化样式表 ##

在脚本化样式表时，将会碰到两类需要使用的对象。

第一类是元素对象，由``<style>``和``<link>``元素表示，两种元素包含或引用样式表。这些是常规的文档元素，如果它们有id属性值，可以用document.getElementById()函数来选择它们。

第二类是CSSStyleSheet对象，它表示样式表本身。document.styleSheets属性是一个只读的类数组对象，它包含CSSStyleSheet对象，表示与文档关联在一起的样式表。如果为定义或引用了样式表的``<style>``或``<link>``元素设置title属性值，该title作为对应CSSStyleSheet对象的title属性就可用。

### 开启和关闭样式表 ###

``<style>、<link>``元素和CSSStyleSheet对象都定义了一个在JavaScript中可以设置和查询的disableed属性，如果disableed属性为true，样式表就被浏览器关闭并忽略。

disableStylesheet()函数，如果传递一个传递数字，函数将其当做document.styleSheets数组中的一个索引，如果传递一个字符串，函数将其当做一个CSS选择器并传递给document.querySelectorAll()，然后设置所有返回元素的disableed属性

### 查询、插入与删除样式表规则 ###

样式表的开启和关闭以外，CSSStyleSheet对象也定义了用来查询、插入和删除样式表规则的API。

document.styleSheets[]数组元素就是CSSStyleSheet对象。CSSStyleSheet对象有一个cssRules[]数组。它包含所有的规则。

cssRules[]和rules[]数组的元素为CSSRules对象。在标准API中，CSSRules包含代表的所有CSS规则，在IE中，rules[]数组只包含样式表中实际存在的样式规则。

CSSRules对象有两个属性可以使用，selectText是规则的css选择器，它引用一个描述与选择器相关联的样式的可写CSSStyleDeclaration对象。CSSStyleDeclaration是用来表示内联和计算样式的相同类型。使用CSSStyleDeclaration对象的cssText属性来获取规则文本表示形式。

除了查询和修改样式表中已存在的规则以外，也能向样式表中添加和从中删除规则。

标准的API接口定义了insertRule()和deleteRule()方法来添加和删除规则，IE中不支持，但定义了大致等效的函数addRule()和removeRule()。

### 创建新样式表 ###

创建整个新样式表并将其添加到文档中是可以能的。在大多数浏览器中，可以用标准DOM技术：只要创建一个全新的``<style>``元素，将其插入到文档头部，然后用其innerHTML属性来设置样式表内容。


