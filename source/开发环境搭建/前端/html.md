HTML（HyperText Markup Language）标签用于定义网页的结构和内容。

[HTML 标签列表（功能排序） | 菜鸟教程](https://www.runoob.com/tags/ref-byfunc.html)
[HTML 标签列表(字母排序) | 菜鸟教程](https://www.runoob.com/tags/html-reference.html)

[HTML 全局属性 | 菜鸟教程](https://www.runoob.com/tags/ref-standardattributes.html)
[HTML 事件 | 菜鸟教程](https://www.runoob.com/tags/ref-eventattributes.html)

# 全局属性
[HTML 全局属性 | 菜鸟教程](https://www.runoob.com/tags/ref-standardattributes.html)

```html
<div class="index-buttonContainer-2FbwvzvBm4ynImHm6txNE-">
    <div class="index-button-1K3MxtEx_SAMX6VKShLDWL"> 一键上传</div>
    <div class="index-button-1K3MxtEx_SAMX6VKShLDWL">下载视频/图片</div>
    <div class="index-buttonRed-1Qwqk5ppUOMviRQYXDH-Xg">加入进货车</div>
</div>
```

| 属性                                                                                                              | 描述                            |
| :-------------------------------------------------------------------------------------------------------------- | :---------------------------- |
| [accesskey](https://www.runoob.com/tags/att-global-accesskey.html "HTML Global accesskey 属性")                   | 设置访问元素的键盘快捷键。                 |
| [class](https://www.runoob.com/tags/att-global-class.html "HTML Global class 属性")                               | 规定元素的类名（classname）            |
| [contenteditable](https://www.runoob.com/tags/att-global-contenteditable.html "HTML Global contenteditable 属性") | 规定是否可编辑元素的内容。                 |
| [contextmenu](https://www.runoob.com/tags/att-global-contextmenu.html "HTML contextmenu 属性")                    | 指定一个元素的上下文菜单。当用户右击该元素，出现上下文菜单 |
| [data-*](https://www.runoob.com/tags/att-global-data.html)                                                      | 用于存储页面的自定义数据                  |
| [dir](https://www.runoob.com/tags/att-global-dir.html "HTML dir 属性")                                            | 设置元素中内容的文本方向。                 |
| [draggable](https://www.runoob.com/tags/att-global-draggable.html "HTML draggable 属性")                          | 指定某个元素是否可以拖动                  |
| [dropzone](https://www.runoob.com/tags/att-global-dropzone.html "HTML dropzone 属性")                             | 指定是否将数据复制，移动，或链接，或删除          |
| [hidden](https://www.runoob.com/tags/att-global-hidden.html "HTML hidden 属性")                                   | hidden 属性规定对元素进行隐藏。           |
| [id](https://www.runoob.com/tags/att-global-id.html "HTML id 属性")                                               | 规定元素的唯一 id                    |
| [lang](https://www.runoob.com/tags/att-global-lang.html "HTML lang 属性")                                         | 设置元素中内容的语言代码。                 |
| [spellcheck](https://www.runoob.com/tags/att-global-spellcheck.html "HTML spellcheck 属性")                       | 检测元素是否拼写错误                    |
| [style](https://www.runoob.com/tags/att-global-style.html "HTML style 属性")                                      | 规定元素的行内样式（inline style）       |
| [tabindex](https://www.runoob.com/tags/att-global-tabindex.html "HTML tabindex 属性")                             | 设置元素的 Tab 键控制次序。              |
| [title](https://www.runoob.com/tags/att-global-title.html "HTML title 属性")                                      | 规定元素的额外信息（可在工具提示中显示）          |
| [translate](https://www.runoob.com/tags/att-global-translate.html "HTML translate 属性")                          | 指定是否一个元素的值在页面载入时是否需要翻译        |

## id
[HTML id 属性 | 菜鸟教程](https://www.runoob.com/tags/att-global-id.html)

```html
<body>
  <div id="app"></div>
</body>
```

### 语法
```html
<_element_ id="_id_">
```

### 属性值

| 值    | 描述                                                                                                                              |
| ---- | ------------------------------------------------------------------------------------------------------------------------------- |
| _id_ | 规定元素的唯一 id。<br><br>命名规则：<br>- 必须以字母 A-Z 或 a-z 开头<br>- 其后的字符：字母(A-Za-z)、数字(0-9)、连字符("-")、下划线("_")、冒号(":") 以及点号(".")<br>- 值对大小写敏感 |


## class
[HTML class 属性 | 菜鸟教程](https://www.runoob.com/tags/att-global-class.html)

```html
<div class="index-detailLine-3PIWlA1zOaMMprbdkxNQ65">
    <div class="index-detailLabel-2eygBLosYtq2jUH88CzmrH">货号</div>
    <div class="index-detailValues-1JNdFTIRzaGkFj-Nlhpqg- index-properiesValue-31gHDGZJNZoaAcprhVh_VL">2514</div>
</div>
```

### 定义

class 属性定义了元素的类名。
class 属性通常用于指向样式表的类。
但是，它也可以用于 JavaScript 中（通过 HTML DOM）, 来修改 HTML 元素的类名。

```html
<div class="index-detailLine-3PIWlA1zOaMMprbdkxNQ65">
    <div class="index-detailLabel-2eygBLosYtq2jUH88CzmrH">货号</div>
    <div class="index-detailValues-1JNdFTIRzaGkFj-Nlhpqg- index-properiesValue-31gHDGZJNZoaAcprhVh_VL">2514</div>
</div>
```

```css
.index-root-Atia1s-X3AFWzPlNIwur9 .index-detailLabel-2eygBLosYtq2jUH88CzmrH {
    margin-right: 8px;
    color: #999;
    white-space: nowrap;
    letter-spacing: 12px
}
```

### 语法

```html
<_element_ class="_classname_">
```

### 属性值

| 值           | 描述                                                                                                                                                                                                   |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _classname_ | 规定元素的类的名称。如需为一个元素规定多个类，用空格分隔类名。 <span class="left important">. HTML 元素允许使用多个类。<br><br>名称规则：<br>- 必须以字母 A-Z 或 a-z 开头<br>- 可以是以下字符： (A-Za-z), 数字 (0-9), 横杆 ("-"), 和 下划线 ("_")<br>- 在 HTML 中, 类名是区分大小写的 |

  





# 事件属性

[HTML 事件 | 菜鸟教程](https://www.runoob.com/tags/ref-eventattributes.html)


## 窗口事件属性（Window Event Attributes）

| 属性                                                                 | 值        | 描述                                     |
| ------------------------------------------------------------------ | -------- | -------------------------------------- |
| [onafterprint](https://www.runoob.com/tags/ev-onafterprint.html)   | _script_ | 在打印文档之后运行脚本                            |
| [onbeforeprint](https://www.runoob.com/tags/ev-onbeforeprint.html) | _script_ | 在文档打印之前运行脚本                            |
| onbeforeonload                                                     | _script_ | 在文档加载之前运行脚本                            |
| onblur                                                             | _script_ | 当窗口失去焦点时运行脚本                           |
| onerror                                                            | _script_ | 当错误发生时运行脚本                             |
| onfocus                                                            | _script_ | 当窗口获得焦点时运行脚本                           |
| onhashchange                                                       | _script_ | 当文档改变时运行脚本                             |
| [onload](https://www.runoob.com/tags/ev-onload.html)               | _script_ | 当文档加载时运行脚本                             |
| onmessage                                                          | _script_ | 当触发消息时运行脚本                             |
| onoffline                                                          | _script_ | 当文档离线时运行脚本                             |
| ononline                                                           | _script_ | 当文档上线时运行脚本                             |
| onpagehide                                                         | _script_ | 当窗口隐藏时运行脚本                             |
| onpageshow                                                         | _script_ | 当窗口可见时运行脚本                             |
| onpopstate                                                         | _script_ | 当窗口历史记录改变时运行脚本                         |
| onredo                                                             | _script_ | 当文档执行再执行操作（redo）时运行脚本                  |
| [onresize](https://www.runoob.com/tags/ev-onresize.html)           | _script_ | 当调整窗口大小时运行脚本                           |
| onstorage                                                          | _script_ | 当 Web Storage 区域更新时（存储空间中的数据发生变化时）运行脚本 |
| onundo                                                             | _script_ | 当文档执行撤销时运行脚本                           |
| [onunload](https://www.runoob.com/tags/ev-onunload.html)           | _script_ | 当用户离开文档时运行脚本                           |

## 表单事件(Form Events)
表单事件在HTML表单中触发 (适用于所有 HTML 元素, 但该HTML元素需在form表单内):

| 属性                                                       | 值        | 描述                     |
| -------------------------------------------------------- | -------- | ---------------------- |
| [onblur](https://www.runoob.com/tags/ev-onblur.html)     | _script_ | 当元素失去焦点时运行脚本           |
| [onchange](https://www.runoob.com/tags/ev-onchange.html) | _script_ | 当元素改变时运行脚本             |
| oncontextmenu                                            | _script_ | 当触发上下文菜单时运行脚本          |
| [onfocus](https://www.runoob.com/tags/ev-onfocus.html)   | _script_ | 当元素获得焦点时运行脚本           |
| onformchange                                             | _script_ | 当表单改变时运行脚本             |
| onforminput                                              | _script_ | 当表单获得用户输入时运行脚本         |
| oninput                                                  | _script_ | 当元素获得用户输入时运行脚本         |
| oninvalid                                                | _script_ | 当元素无效时运行脚本             |
| onreset                                                  | _script_ | 当表单重置时运行脚本。HTML 5 不支持。 |
| [onselect](https://www.runoob.com/tags/ev-onselect.html) | _script_ | 当选取元素时运行脚本             |
| [onsubmit](https://www.runoob.com/tags/ev-onsubmit.html) | _script_ | 当提交表单时运行脚本             |

## 键盘事件（Keyboard Events）

| 属性                                                           | 值        | 描述            |
| ------------------------------------------------------------ | -------- | ------------- |
| [onkeydown](https://www.runoob.com/tags/ev-onkeydown.html)   | _script_ | 当按下按键时运行脚本    |
| [onkeypress](https://www.runoob.com/tags/ev-onkeypress.html) | _script_ | 当按下并松开按键时运行脚本 |
| [onkeyup](https://www.runoob.com/tags/ev-onkeyup.html)       | _script_ | 当松开按键时运行脚本    |
## 鼠标事件（Mouse Events）

通过鼠标触发事件, 类似用户的行为:

| 属性                                                             | 值        | 描述                   |
| -------------------------------------------------------------- | -------- | -------------------- |
| [onclick](https://www.runoob.com/tags/ev-onclick.html)         | _script_ | 当单击鼠标时运行脚本           |
| [ondblclick](https://www.runoob.com/tags/ev-ondblclick.html)   | _script_ | 当双击鼠标时运行脚本           |
| ondrag                                                         | _script_ | 当拖动元素时运行脚本           |
| ondragend                                                      | _script_ | 当拖动操作结束时运行脚本         |
| ondragenter                                                    | _script_ | 当元素被拖动至有效的拖放目标时运行脚本  |
| ondragleave                                                    | _script_ | 当元素离开有效拖放目标时运行脚本     |
| ondragover                                                     | _script_ | 当元素被拖动至有效拖放目标上方时运行脚本 |
| ondragstart                                                    | _script_ | 当拖动操作开始时运行脚本         |
| ondrop                                                         | _script_ | 当被拖动元素正在被拖放时运行脚本     |
| [onmousedown](https://www.runoob.com/tags/ev-onmousedown.html) | _script_ | 当按下鼠标按钮时运行脚本         |
| [onmousemove](https://www.runoob.com/tags/ev-onmousemove.html) | _script_ | 当鼠标指针移动时运行脚本         |
| [onmouseout](https://www.runoob.com/tags/ev-onmouseout.html)   | _script_ | 当鼠标指针移出元素时运行脚本       |
| [onmouseover](https://www.runoob.com/tags/ev-onmouseover.html) | _script_ | 当鼠标指针移至元素之上时运行脚本     |
| [onmouseup](https://www.runoob.com/tags/ev-onmouseup.html)     | _script_ | 当松开鼠标按钮时运行脚本         |
| onmousewheel                                                   | _script_ | 当转动鼠标滚轮时运行脚本         |
| onscroll                                                       | _script_ | 当滚动元素的滚动条时运行脚本       |

## 多媒体事件(Media Events)

通过视频（videos），图像（images）或者音频（audio） 触发该事件，多应用于 HTML 媒体元素

| 属性                 | 值        | 描述                             |
| ------------------ | -------- | ------------------------------ |
| onabort            | _script_ | 当发生中止事件时运行脚本                   |
| oncanplay          | _script_ | 当媒介能够开始播放但可能因缓冲而需要停止时运行脚本      |
| oncanplaythrough   | _script_ | 当媒介能够无需因缓冲而停止即可播放至结尾时运行脚本      |
| ondurationchange   | _script_ | 当媒介长度改变时运行脚本                   |
| onemptied          | _script_ | 当媒介资源元素突然为空时（网络错误、加载错误等）运行脚本   |
| onended            | _script_ | 当媒介已抵达结尾时运行脚本                  |
| onerror            | _script_ | 当在元素加载期间发生错误时运行脚本              |
| onloadeddata       | _script_ | 当加载媒介数据时运行脚本                   |
| onloadedmetadata   | _script_ | 当媒介元素的持续时间以及其他媒介数据已加载时运行脚本     |
| onloadstart        | _script_ | 当浏览器开始加载媒介数据时运行脚本              |
| onpause            | _script_ | 当媒介数据暂停时运行脚本                   |
| onplay             | _script_ | 当媒介数据将要开始播放时运行脚本               |
| onplaying          | _script_ | 当媒介数据已开始播放时运行脚本                |
| onprogress         | _script_ | 当浏览器正在取媒介数据时运行脚本               |
| onratechange       | _script_ | 当媒介数据的播放速率改变时运行脚本              |
| onreadystatechange | _script_ | 当就绪状态（ready-state）改变时运行脚本      |
| onseeked           | _script_ | 当媒介元素的定位属性 [1] 不再为真且定位已结束时运行脚本 |
| onseeking          | _script_ | 当媒介元素的定位属性为真且定位已开始时运行脚本        |
| onstalled          | _script_ | 当取回媒介数据过程中（延迟）存在错误时运行脚本        |
| onsuspend          | _script_ | 当浏览器已在取媒介数据但在取回整个媒介文件之前停止时运行脚本 |
| ontimeupdate       | _script_ | 当媒介改变其播放位置时运行脚本                |
| onvolumechange     | _script_ | 当媒介改变音量亦或当音量被设置为静音时运行脚本        |
| onwaiting          | _script_ | 当媒介已停止播放但打算继续播放时运行脚本           |

## 其他事件

| 属性                                                       | 值        | 描述                       |
| -------------------------------------------------------- | -------- | ------------------------ |
| [onshow](https://www.runoob.com/tags/ev-onshow.html)     | _script_ | 当 <menu> 元素在上下文显示时触发     |
| [ontoggle](https://www.runoob.com/tags/ev-ontoggle.html) | _script_ | 当用户打开或关闭 <details> 元素时触发 |

# 标签分类
[HTML 标签列表（功能排序） | 菜鸟教程](https://www.runoob.com/tags/ref-byfunc.html)

## 基础
### DOCTYPE

### html

### title

### body

### h1 ~ h6

### p

### hr


## 元信息

### head

### meta

### base





## 样式/节

### header

### footer

### section

### div

### span

### article

### aside

### dialog


### details

### summary

### style


## 链接

### link

### a
[HTML \<a\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-a.html)

```html
<a href="https://www.runoob.com">访问菜鸟教程!</a>
```

| 属性                                                                               | 值                                                                                                                                               | 描述                                                                                                                                   |
| :------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| [charset](https://www.runoob.com/tags/att-a-charset.html "HTML a charset 属性")    | _char_encoding_                                                                                                                                 | HTML5 不支持。规定目标 URL 的字符编码。                                                                                                            |
| [coords](https://www.runoob.com/tags/att-a-coords.html "HTML a coords 属性")       | _coordinates_                                                                                                                                   | HTML5 不支持。规定链接的坐标。                                                                                                                   |
| [download](https://www.runoob.com/tags/att-a-download.html "HTML a download 属性") | _filename_                                                                                                                                      | 指定下载链接                                                                                                                               |
| [href](https://www.runoob.com/tags/att-a-href.html "HTML a href 属性")             | _URL_                                                                                                                                           | 规定链接的目标 URL。                                                                                                                         |
| [hreflang](https://www.runoob.com/tags/att-a-hreflang.html "HTML a hreflang属性")  | _language_code_                                                                                                                                 | 规定目标 URL 的基准语言。仅在 href 属性存在时使用。                                                                                                      |
| [media](https://www.runoob.com/tags/att-a-media.html "HTML a media属性")           | _media_query_                                                                                                                                   | 规定目标 URL 的媒介类型。默认值：all。仅在 href 属性存在时使用。                                                                                              |
| [name](https://www.runoob.com/tags/att-a-name.html "HTML a name属性")              | _section_name_                                                                                                                                  | HTML5 不支持。规定锚的名称。                                                                                                                    |
| [rel](https://www.runoob.com/tags/att-a-rel.html "HTML a rel属性")                 | alternate  <br>author  <br>bookmark  <br>help  <br>license  <br>next  <br>nofollow  <br>noreferrer  <br>prefetch  <br>prev  <br>search  <br>tag | 规定当前文档与目标 URL 之间的关系。仅在 href 属性存在时使用。                                                                                                 |
| [rev](https://www.runoob.com/tags/att-a-rev.html "HTML a rev属性")                 | _text_                                                                                                                                          | HTML5 不支持。规定目标 URL 与当前文档之间的关系。                                                                                                       |
| [shape](https://www.runoob.com/tags/att-a-shape.html "HTML a shape属性")           | default  <br>rect  <br>circle  <br>poly                                                                                                         | HTML5 不支持。规定链接的形状。                                                                                                                   |
| [target](https://www.runoob.com/tags/att-a-target.html "HTML a target属性")        | _blank  <br>_parent  <br>_self  <br>_top  <br>_framename_                                                                                       | 规定在何处打开目标 URL。仅在 href 属性存在时使用。<br>- _blank：新窗口打开。<br>- _parent：在父窗口中打开链接。<br>- _self：默认，当前页面跳转。<br>- _top：在当前窗体打开链接，并替换当前的整个窗体(框架页)。 |
| [type](https://www.runoob.com/tags/att-a-type.html "HTML a type属性")              | _MIME_type_                                                                                                                                     | 规定目标 URL 的 MIME 类型。仅在 href 属性存在时使用。  <br>注：MIME = Multipurpose Internet Mail Extensions。                                             |


### main

### nav



## 图像

### img

[HTML \<img\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-img.html)

```html
<img src="smiley-2.gif" alt="Smiley face" width="42" height="42">
```

| 属性                                                            | 值                                                | 描述                                          |
| :------------------------------------------------------------ | :----------------------------------------------- | :------------------------------------------ |
| [align](https://www.runoob.com/tags/att-img-align.html)       | top  <br>bottom  <br>middle  <br>left  <br>right | HTML5 不支持。HTML 4.01 已废弃。 规定如何根据周围的文本来排列图像。  |
| [loading](https://www.runoob.com/tags/att-img-loading.html)   | eager：立即加载  <br>lazy：延迟加载                        | 指定浏览器是应立即加载图像还是延迟加载图像。                      |
| [alt](https://www.runoob.com/tags/att-img-alt.html)           | _text_                                           | 规定图像的替代文本。                                  |
| [border](https://www.runoob.com/tags/att-img-border.html)     | _pixels_                                         | HTML5 不支持。HTML 4.01 已废弃。 规定图像周围的边框。         |
| crossorigin                                                   | anonymous  <br>use-credentials                   | 设置图像的跨域属性                                   |
| [height](https://www.runoob.com/tags/att-img-height.html)     | _pixels_                                         | 规定图像的高度。                                    |
| [hspace](https://www.runoob.com/tags/att-img-hspace.html)     | _pixels_                                         | HTML5 不支持。HTML 4.01 已废弃。 规定图像左侧和右侧的空白。      |
| [ismap](https://www.runoob.com/tags/att-img-ismap.html)       | ismap                                            | 将图像规定为服务器端图像映射。                             |
| [longdesc](https://www.runoob.com/tags/att-img-longdesc.html) | _URL_                                            | HTML5 不支持。HTML 4.01 已废弃。 指向包含长的图像描述文档的 URL。 |
| [src](https://www.runoob.com/tags/att-img-src.html)           | _URL_                                            | 规定显示图像的 URL。                                |
| [usemap](https://www.runoob.com/tags/att-img-usemap.html)     | _#mapname_                                       | 将图像定义为客户器端图像映射。                             |
| [vspace](https://www.runoob.com/tags/att-img-vspace.html)     | _pixels_                                         | HTML5 不支持。HTML 4.01 已废弃。 规定图像顶部和底部的空白。      |
| [width](https://www.runoob.com/tags/att-img-width.html)       | _pixels_                                         | 规定图像的宽度。                                    |

### map

### area

### canvas

### figcaption

### figure



## 格式



## 列表

## 表格

## 表单

## 框架


## Audio/Video


## 程序	




# 常见标签
## 定义注释
[HTML \<!–…–\> 注释标签 | 菜鸟教程](https://www.runoob.com/tags/tag-comment.html)

```html
<!-- Import Vue 3 -->
<!-- Import component library -->
<!--这是一个注释，注释在浏览器中不会显示-->
```

## doctype - 定义文档类型
[Fetching Title#dl4y](https://www.runoob.com/tags/tag-doctype.html)

```html
<!DOCTYPE html>

<html>
<head>
<meta charset="utf-8"> 
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```


## html - 定义 HTML 文档

[HTML \<html\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-html.html)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文档标题</title>
</head>

<body>
文档内容......
</body>

</html>
```

```html
<html> 标签告知浏览器这是一个 HTML 文档。
<html> 标签是 HTML 文档中最外层的元素。
<html> 标签是所有其他 HTML 元素的容器。
```

## # head - 定义关于文档的信息
[HTML \<head\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-head.html)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```

```html
<head> 元素是所有头部元素的容器。
<head> 元素必须包含文档的标题（title），可以包含脚本、样式、meta 信息 以及其他更多的信息。

以下列出的元素能被用在 <head> 元素内部：
- <title> （在头部中，这个元素是必需的）
- <style>
- <base>
- <link>
- <meta>
- <script>
- <noscript>
```

### title - 定义文档的标题

[HTML \<title\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-title.html)

```html
  <head>
    <title>Vite + Vue + TS</title>
  </head>
```


### meta - 定义关于 HTML 文档的元信息

[HTML \<meta\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-meta.html)
```html
<head>
<meta name="description" content="免费在线教程">
<meta name="keywords" content="HTML,CSS,XML,JavaScript">
<meta name="author" content="runoob">
<meta charset="UTF-8">
</head>
```

```html
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
```

```html
<head>
  <meta content="text/html; charset=utf-8" http-equiv="content-type">
</head>
```

| 属性                                                                 | 值                                                                    | 描述                               |
| :----------------------------------------------------------------- | :------------------------------------------------------------------- | :------------------------------- |
| [charset](https://www.runoob.com/tags/att-meta-charset.html)       | _character_set_                                                      | 定义文档的字符编码。                       |
| [content](https://www.runoob.com/tags/att-meta-content.html)       | _text_                                                               | 定义与 http-equiv 或 name 属性相关的元信息。  |
| [http-equiv](https://www.runoob.com/tags/att-meta-http-equiv.html) | content-type  <br>default-style  <br>refresh                         | 把 content 属性关联到 HTTP 头部。         |
| [name](https://www.runoob.com/tags/att-meta-name.html)             | application-name  <br>author  <br>description  <br>generatorkeywords | 把 content 属性关联到一个名称。             |
| [scheme](https://www.runoob.com/tags/att-meta-scheme.html)         | _format/URI_                                                         | HTML5不支持。 定义用于翻译 content 属性值的格式。 |

#### name
[HTML meta name 属性 | 菜鸟教程](https://www.runoob.com/tags/att-meta-name.html)

```html
<head>
	<meta name="description" content="Free Web tutorials">
	<meta name="keywords" content="HTML,CSS,JavaScript">
	<meta name="author" content="Hege Refsnes">
</head>
```

| 值                | 描述                                                                                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application-name | 规定页面所代表的 Web 应用程序的名称。                                                                                                                                               |
| author           | 规定文档的作者的名字。  <br>  <br>实例： <meta name="author" content="Hege Refsnes">                                                                                              |
| description      | 规定页面的描述。搜索引擎会把这个描述显示在搜索结果中。  <br>  <br>实例： <meta name="description" content="Free web tutorials">                                                                   |
| generator        | 规定用于生成文档的一个软件包（不用于手写页面）。  <br>  <br>实例： <meta name="generator" content="FrontPage 4.0">                                                                             |
| keywords         | 规定一个逗号分隔的关键词列表 - 相关的网页（告诉搜索引擎页面是与什么相关的）。  <br>  <br>**提示：**总是规定关键词（对于搜索引擎进行页面分类是必要的）。  <br>  <br>实例： <meta name="keywords" content="HTML, meta tag, tag reference"> |

#### charset
[Site Unreachable](https://www.runoob.com/tags/att-meta-charset.html)

```html
<head>
	<meta charset="UTF-8">
</head>
```

| 值               | 描述                                                                                                                                                                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _character_set_ | 规定 HTML 文档的字符编码。<br><br>常用的值：<br><br>- UTF-8 - Unicode 字符编码<br>- ISO-8859-1 - 拉丁字母表的字符编码<br><br>在理论上，可以使用任何字符编码，但并不是所有浏览器都能够理解它们。某种字符编码使用的范围越广，浏览器就越有可能理解它。<br><br>如需查看所有可用的字符编码，请访问 [IANA 字符集](http://www.iana.org/assignments/character-sets)。 |

#### content
[HTML meta content 属性 | 菜鸟教程](https://www.runoob.com/tags/att-meta-content.html)

```html
<head>
	<meta name="description" content="Free Web tutorials">
	<meta name="keywords" content="HTML,CSS,XML,JavaScript">
</head>
```

| 值      | 描述          |
| ------ | ----------- |
| _text_ | meta 信息的内容。 |

#### http-equiv
[HTML meta http-equiv 属性 | 菜鸟教程](https://www.runoob.com/tags/att-meta-http-equiv.html)

```html
<head>
	<meta http-equiv="refresh" content="30">
</head>
```

| 值             | 描述                                                                                                                                                                                                                      |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| content-type  | 规定文档的字符编码。<br><br>实例：<br><br><meta http-equiv="content-type" content="text/html; charset=UTF-8">                                                                                                                        |
| default-style | 规定要使用的预定义的样式表。<br><br>实例：<br><br><meta http-equiv="default-style" content="_the document's preferred stylesheet_"><br><br>**注释：**上面 content 属性的值必须匹配同一文档中的一个 link 元素上的 title 属性的值，或者必须匹配同一文档中的一个 style 元素上的 title 属性的值。 |
| refresh       | 定义文档自动刷新的时间间隔。<br><br>实例：<br><br><meta http-equiv="refresh" content="300"><br><br>注释：值 "refresh" 应该慎重使用，因为它会使得页面不受用户控制。在 [W3C's Web 内容可访问性指南](http://www.w3.org/WAI/intro/wcag.php) 中使用 "refresh" 会到导致失败。               |



### link - 定义文档与外部资源的关系

[HTML \<link\>标签 | 菜鸟教程](https://www.runoob.com/tags/tag-link.html)

```html
<link rel="stylesheet" href="/style/element-plus@2.3.8.css" />
```

```html
  <head>
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
  </head>
```

| 属性                                                             | 值                                                                                                                                                                                                                                                             | 描述                                            |
| :------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------- |
| [charset](https://www.runoob.com/tags/att-link-charset.html)   | _char_encoding_                                                                                                                                                                                                                                               | HTML5 不支持该属性。 定义被链接文档的字符编码方式。                 |
| [href](https://www.runoob.com/tags/att-link-href.html)         | _URL_                                                                                                                                                                                                                                                         | 定义被链接文档的位置。                                   |
| [hreflang](https://www.runoob.com/tags/att-link-hreflang.html) | _language_code_                                                                                                                                                                                                                                               | 定义被链接文档中文本的语言。                                |
| [media](https://www.runoob.com/tags/att-link-media.html)       | _media_query_                                                                                                                                                                                                                                                 | 规定被链接文档将显示在什么设备上。                             |
| [rel](https://www.runoob.com/tags/att-link-rel.html)           | alternate  <br>archives  <br>author  <br>bookmark  <br>external  <br>first  <br>help  <br>icon  <br>last  <br>license  <br>next  <br>nofollow  <br>noreferrer  <br>pingback  <br>prefetch  <br>prev  <br>search  <br>sidebar  <br>stylesheet  <br>tag  <br>up | 必需。定义当前文档与被链接文档之间的关系。rel 是 relationship的英文缩写。 |
| [rev](https://www.runoob.com/tags/att-link-rev.html)           | _reversed relationship_                                                                                                                                                                                                                                       | HTML5 不支持该属性。 定义被链接文档与当前文档之间的关系。              |
| [sizes](https://www.runoob.com/tags/att-link-sizes.html)       | _Height_x_Width  <br>_any                                                                                                                                                                                                                                     | 定义了链接属性大小，只对属性 rel="icon" 起作用。                |
| [target](https://www.runoob.com/tags/att-link-target.html)     | _blank  <br>_self  <br>_top  <br>_parent  <br>_frame_name_                                                                                                                                                                                                    | HTML5 不支持该属性。 定义在何处加载被链接文档。                   |
| [type](https://www.runoob.com/tags/att-link-type.html)         | _MIME_type_                                                                                                                                                                                                                                                   | 规定被链接文档的 MIME 类型。                             |
#### charset
[HTML link charset 属性 | 菜鸟教程](https://www.runoob.com/tags/att-link-charset.html)
```html
<link href="domoarigato.htm" rel="parent" charset="ISO-2022-JP">
```

| 值               | 描述                                                                                                                                                                                                                                                   |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _character_set_ | 所链接文档的字符编码。<br><br>常用的字符集有：<br><br>- UTF-8 - Unicode 字符编码<br>- ISO-8859-1 - 拉丁字母表的字符编码<br><br>在理论上，可以使用任何字符编码，但并不是所有浏览器都能够理解它们。某种字符编码使用的范围越广，浏览器就越有可能理解它。<br><br>如需查看所有可用的字符编码，请访问我们的 [字符集参考手册](https://www.runoob.com/tags/ref-charactersets.html)。 |
#### rel
[Site Unreachable](https://www.runoob.com/tags/att-link-rel.html)

```html
<link rel="stylesheet" type="text/css" href="theme.css">
```

| 值          | 描述                               |
| ---------- | -------------------------------- |
| alternate  | 链接到该文档的替代版本（比如打印页、翻译或镜像）。        |
| author     | 链接到该文档的作者。                       |
| help       | 链接到帮助文档。                         |
| icon       | 导入表示该文档的图标。                      |
| license    | 链接到该文档的版权信息。                     |
| next       | 表示该文档是集合中的一部分，且集合中的下一个文档是被引用的文档。 |
| prefetch   | 规定应该对目标资源进行缓存。                   |
| prev       | 表示该文档是集合中的一部分，且集合中的上一个文档是被引用的文档。 |
| search     | 链接到针对文档的搜索工具。                    |
| stylesheet | 要导入的样式表的 URL。                    |


#### type

[HTML link type 属性 | 菜鸟教程](https://www.runoob.com/tags/att-link-type.html)

```html
<head>
	<link rel="stylesheet" type="text/css" href="theme.css">
</head>
```

| 值           | 描述                                                                                                       |
| ----------- | -------------------------------------------------------------------------------------------------------- |
| _MIME_type_ | 被链接文档的 MIME 类型。  <br>请参阅 [IANA MIME 类型](http://www.iana.org/assignments/media-types/)，获得标准 MIME 类型的完整列表。 |


#### href
[Site Unreachable](https://www.runoob.com/tags/att-link-href.html)

```html
<link rel="stylesheet" type="text/css" href="theme.css">
```

| 值     | 描述                                                                                                                                                  |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| _URL_ | 被链接资源/文档的 URL。<br><br>可能的值：<br>- 绝对 URL - 指向另一个网站（比如 href="http://www.example.com/theme.css"）<br>- 相对 URL - 指向网站内的一个文件（比如 href="/themes/theme.css"） |

#### crossorigin
[HTML \<script\> crossorigin 属性](https://www.w3school.com.cn/tags/att_script_crossorigin.asp)

```html
  <head>
    <link rel="stylesheet" crossorigin href="/assets/index-M1QJLWO9.css">
  </head>
```

| 值                                | 描述                                                                                                              |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| - anonymous<br>- use-credentials | 规定 CORS 请求的模式：<br><br>- anonymous - 执行跨源请求。不发送凭据。<br>- use-credentials - 执行跨源请求。发送凭据（例如：cookie、证书、HTTP 基本身份验证）。 |

### script - 定义客户端脚本
[HTML \<script\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-script.html)

```html
<head>
	<script type="module" crossorigin src="/assets/index-DycvRFTd.js"></script>
</head>
```

```html
<script>
document.write("Hello World!")
</script>
```

```html
<head>
  <script src="/js/vue@3.3.4.global.js"></script>
  <script src="/js/element-plus@2.3.8.full.js"></script>
  <script src="/js/icons-vue@2.1.0.iife.js"></script>
  <script src="/js/vue-router@4.2.4.global.js"></script>
  <script src="/js/lodash@4.17.21.lodash.js"></script>
  <script src="/js/axios@1.4.0.js"></script>
</head>
```

| 属性                                                             | 值           | 描述                          |
| :------------------------------------------------------------- | :---------- | :-------------------------- |
| [async](https://www.runoob.com/tags/att-script-async.html)     | async       | 规定异步执行脚本（仅适用于外部脚本）。         |
| [charset](https://www.runoob.com/tags/att-script-charset.html) | _charset_   | 规定在脚本中使用的字符编码（仅适用于外部脚本）。    |
| [defer](https://www.runoob.com/tags/att-script-defer.html)     | defer       | 规定当页面已完成解析后，执行脚本（仅适用于外部脚本）。 |
| [src](https://www.runoob.com/tags/att-script-src.html)         | _URL_       | 规定外部脚本的 URL。                |
| [type](https://www.runoob.com/tags/att-script-type.html)       | _MIME-type_ | 规定脚本的 MIME 类型。              |
| xml:space                                                      | preserve    | HTML5 不支持。规定是否保留代码中的空白。     |

#### type
[HTML script type 属性 | 菜鸟教程](https://www.runoob.com/tags/att-script-type.html)

```html
<script type="text/javascript">
	document.write("Hello World!");
</script>
```

| 值           | 描述                                                                                                                                                                                                                                                          |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _MIME_type_ | 规定脚本的 MIME 类型。  <br>  <br>一些常用的值：<br><br>- text/javascript （默认）<br>- text/ecmascript<br>- application/ecmascript<br>- application/javascript<br>- text/vbscript<br><br>请参阅 [IANA MIME 类型](http://www.iana.org/assignments/media-types/) ，获得标准 MIME 类型的完整列表。 |

#### src
[HTML script src 属性 | 菜鸟教程](https://www.runoob.com/tags/att-script-src.html)

```html
<script src="myscripts.js"></script>
```

##### 语法
```html
<script src="_URL_">
```

##### 属性值

| 值     | 描述                                                                                                                                                 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| _URL_ | 外部脚本文件的 URL。<br><br>可能的值：<br>- 绝对 URL - 指向另一个网站（比如 src="http://www.example.com/example.js"）<br>- 相对 URL - 指向网站内的一个文件（比如 src="/scripts/example.js"） |

## body - 定义文档的主体

[HTML \<body\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-body.html)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>文档标题</title>
</head>
 
<body>
文档内容......
</body>
 
</html>
```

### div
[HTML \<div\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-div.html)

```html
<div style="color:#0000FF">
  <h3>这是一个在 div 元素中的标题。</h3>
  <p>这是一个在 div 元素中的文本。</p>
</div>
```

```html
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
  </body>
```

```html
<div> 标签定义 HTML 文档中的一个分隔区块或者一个区域部分。
<div>   标签常用于组合块级元素，以便通过 CSS 来对这些元素进行格式化。
<div>   元素经常与 CSS 一起使用，用来布局网页。
```

### script
[HTML \<script\> 标签 | 菜鸟教程](https://www.runoob.com/tags/tag-script.html)

```html
<script>
document.write("Hello World!")
</script>
```

| 属性                                                             | 值           | 描述                          |
| :------------------------------------------------------------- | :---------- | :-------------------------- |
| [async](https://www.runoob.com/tags/att-script-async.html)     | async       | 规定异步执行脚本（仅适用于外部脚本）。         |
| [charset](https://www.runoob.com/tags/att-script-charset.html) | _charset_   | 规定在脚本中使用的字符编码（仅适用于外部脚本）。    |
| [defer](https://www.runoob.com/tags/att-script-defer.html)     | defer       | 规定当页面已完成解析后，执行脚本（仅适用于外部脚本）。 |
| [src](https://www.runoob.com/tags/att-script-src.html)         | _URL_       | 规定外部脚本的 URL。                |
| [type](https://www.runoob.com/tags/att-script-type.html)       | _MIME-type_ | 规定脚本的 MIME 类型。              |
| xml:space                                                      | preserve    | HTML5 不支持。规定是否保留代码中的空白。     |
