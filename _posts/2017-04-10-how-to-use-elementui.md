---
layout: post
title:  "ElementUI快速入门"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/elementui_icon.png)

## 一、概述

> Element，是一套为开发者、设计师和产品经理准备的基于Vue 2.0的组件库，提供了配套设计资源，帮助网站快速成型。

[官网地址](http://element.eleme.io/#/zh-CN)

## 二、安装

为了能够更好地和webpack配合使用，使用npm进行安装：

    npm i element-ui --save-dev

## 三、引入

可以引入整个ElementUI。在main.js文件中引入：

	import ElementUI from 'element-ui'
    import 'element-ui/lib/theme-default/index.css'
	Vue.use(ElementUI)

也可以按需引入部分组件。安装babel插件：

    npm install babel-plugin-component --save-dev

修改.babelrc文件：

    {
      "presets": [
        ["es2015", { "modules": false }]
      ],
      "plugins": [["component", [
        {
          "libraryName": "element-ui",
          "styleLibraryName": "theme-default"
        }
      ]]]
    }

在main.js文件中引入部分组件：

    import { Button, Select } from 'element-ui'
    Vue.use(Button)
    Vue.use(Select)

## 四、用法

### 1、基础组件

#### 1.1 布局
通过基础的24分栏，可以迅速简便地创建布局。布局元素主要有两个：

* `el-row`，行

    属性gutter用来设置栅格之间的间隔

    属性type设置为flex时，可以结合属性justify或align进行水平或垂直方向上的对齐

* `el-col`，列

    属性span用来设置栅格所占的列数

    属性offset用来设置栅格的偏移

    属性push或pull可以向右或向左移动栅格

    属性xs、sm、md、lg可以进行响应式布局

#### 1.2 色彩
为了避免视觉传达差异，使用一套特定的调色板来规定颜色，为所搭建的产品提供一致的外观视觉感受。

* 主色

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/01-color01.png)

* 辅助色

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/01-color02.png)

* 中性色

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/01-color03.png)


#### 1.3 字体
对字体进行统一规范，力求在各个操作系统下都有最佳展示效果。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/02-font01.png)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/02-font02.png)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/02-font03.png)

#### 1.4 图标
提供了一套常用的图标集合，直接通过设置类名为el-icon-iconName来使用即可。

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/03-icon01.png)

![](https://github.com/gefenghua/MarkdownPictures/raw/master/2017-04-10-how-to-use-elementui/03-icon02.png)

#### 1.5 按钮
按钮元素主要有两个：

* `el-button-group`，按钮组

* `el-button`，按钮

    属性type用来设置主题

    属性disabled用来设置按钮是否禁用，默认为false

    属性autofocus用来设置按钮是否自动聚焦，默认为false

    属性icon用来设置图标按钮，值可参照图标名

    属性size用来设置按钮的尺寸，值可以为large/small/mini

7种主题：

* 默认为default
* text为文字按钮
* primary为主要按钮
* info为信息按钮
* success为成功按钮
* warning为警告按钮
* danger为危险按钮

### 2、表单组件

#### 2.1 单选框
在一组备选项中进行单选。

* `el-radio-group`，单选框组

    属性size用来设置单选框组的尺寸，值可以为large/small

    属性fill用来设置按钮激活时的填充色和边框色，默认为#20a0ff

    属性text-color用来设置按钮激活时的文本颜色，默认为#ffffff

* `el-radio`，单选框

    属性label用来设置选项的值，当数据对象的值等于选项的值，表示该选项被选中

    属性disabled用来设置是否禁用，默认为false

#### 2.2 多选框
在一组备选项中进行多选。

* `el-checkbox-group`，多选框组

* `el-checkbox`，多选框

    属性label用来设置选中状态的值

    属性true-label和false-label用来设置选中时和没有选中时的值

    属性disabled用来设置是否禁用，默认为false

    属性checked用来判断选项当前是否被选中，默认为false

    属性indeterminate用来实现全选效果，默认为false

#### 2.3 输入框
通过鼠标或键盘输入字符。

* `el-input`，输入框

    属性type用来设置输入框的类型，值可以为text/textarea，默认为text

    属性value用来设置输入框的值

    属性minlength和maxlength用来设置输入框的最小和最大输入长度

    属性placeholder用来设置输入框的占位文本

    属性disabled用来设置是否禁用，默认为false

    属性size用来设置输入框的尺寸，只在type为text时有效，值可以为large/small/mini

    属性icon用来设置输入框尾部图标

    属性rows用来设置输入框的行数，只在type为textarea时有效

    属性resize用来设置能否被缩放，值可以为none/both/horizontal/vertical

    事件click在点击输入框内的图标时触发

    事件blur在输入框失去焦点时触发

    事件focus在输入框获得焦点时触发

    事件change在输入框的值发生改变时触发

#### 2.4 计数器
仅允许输入标准的数字值，可定义范围。

* `el-input-number`，计数器

    属性value用来设置计数器的值

    属性min和max用来设置计数器允许的最小值和最大值

    属性step用来设置计数器的步长，默认为1

    属性size用来设置计数器的尺寸，值可以为large/small

    属性disabled用来设置计数器是否禁用，默认为false

    属性controls用来设置是否使用控制按钮，默认为true

#### 2.5 选择器
当选项过多时，使用下拉菜单展示并选择内容。

* `el-select`，下拉框

    属性disabled用来设置是否禁用，默认为false

    属性size用来设置输入框尺寸，值可以为large/small/mini

    属性multiple用来设置是否可以多选，默认为false

    属性multiple-limit用来设置多选时可以选择的项目数，默认为0，即无限制

    属性clearable用来设置单选时是否清空选项，默认为false

    属性placeholder用来设置输入框的占位文字

    属性filterable用来设置是否可以搜索，默认为false

    事件change在选中值发生改变时触发，参数为当前的选中值

* `el-option`，下拉框选项

    属性value用来设置选项的值

    属性label用来设置选项的标签，默认与value值相同

    属性disabled用来设置是否禁用，默认为false

#### 2.6 开关
表示两种相互对立的状态间的切换，多用于触发「开/关」。

* `el-switch`，开关

    属性disabled用来设置是否禁用开关，默认为false

    属性width用来设置开关的宽度，单位为像素

    属性on-text和off-text用来设置开关的文字描述

    属性on-color和off-color用来设置开关的背景色

    属性on-icon-class和off-icon-class用来设置开关所显示图标的类名，若设置此项会忽略on-text和off-text

    事件change在开关状态发生改变时触发，参数为新状态的布尔值

#### 2.7 滑块
通过拖动滑块在一个固定区间内进行选择。

* `el-slider`，滑块

    属性min和max用来设置滑块的最小值和最大值

    属性disabled用来设置是否禁用滑块，默认为false

    属性step用来设置滑块的步长，默认为1

    属性show-input用来设置是否显示输入框，默认为false

    属性show-input-controls用来设置在显示输入框的情况下，是否显示输入框的控制按钮，默认为true

    属性show-stops用来设置是否显示间断点，默认为false

    属性range用来设置是否为范围选择，默认为false

#### 2.8 日期选择器
用于选择或输入日期。

* `el-time-picker`，日期选择器

    属性type用来设置显示类型，值可以为year/month/date/week/datetime/datetimerange/daterange，默认为date

    属性formate用来格式化日期格式，默认为yyyy-MM-dd

    属性align用来设置对齐方式，值可以为left/center/right，默认为left

    属性size用来设置输入框尺寸，值可以为large/small/mini

    属性clearable用来设置是否显示清除按钮，默认为true

    属性disabled用来设置是否禁用，默认为false

    属性editable用来设置文本框是否可以输入，默认为true

    属性readable用来设置是否只读，默认为false

    属性range-separator用来设置选择范围时的分隔符，默认为‘-’

    属性picker-options用来设置日期选择器的选项

    事件change在输入值发生改变时触发，参数为格式化后的值，返回值和文本框一致

* picker-options的值：

    shortcuts用来设置快捷选项，需要传入{text, onClick}。其中text为标题文本，onClick为选中后的回调函数，参数是vm，可以通过触发pick事件设置选择器的值，vm.$emit('pick', new Date())

    firstDayOfWeek用来设置周起始日，默认为7

#### 2.9 时间选择器
用于选择或输入时间。

* `el-time-select`，时间选择器

    属性align用来设置对齐方式，值可以为left/center/right，默认为left

    属性size用来设置输入框尺寸，值可以为large/small/mini

    属性clearable用来设置是否显示清除按钮，默认为true

    属性disabled用来设置是否禁用，默认为false

    属性editable用来设置文本框是否可以输入，默认为true

    属性readable用来设置是否只读，默认为false

    属性range-separator用来设置选择范围时的分隔符，默认为‘-’

    属性picker-options用来设置日期选择器的选项

    事件change在输入值发生改变时触发，参数为格式化后的值，返回值和文本框一致

* picker-options的值：

    start用来设置开始时间，默认为09:00

    end用来设置结束时间，默认为18:00

    step用来设置间隔时间，默认为00:30

    minTime和maxTime用来设置最小时间和最大时间

#### 2.10 上传
通过点击或者拖拽上传文件。

* `el-upload`，上传

    属性action用来设置上传的地址，必选

    属性headers用来设置上传的请求头部

    属性multiple用来设置是否支持多选文件

    属性data用来设置上传时附带的参数

    属性name用来设置上传的文件字段名，默认为file

    属性show-file-list用来设置是否显示已上传文件列表，默认为true

    属性file-list用来设置上传的文件列表

    属性type用来设置上传控件类型，值可以为select/drag，默认为select

    属性accept用来设置接受上传的文件类型

    属性list-type用来设置文件列表的类型，值可以为text/picture/picture-card，默认为text

    属性on-preview用来设置点击已上传文件链接时的钩子函数

    属性on-remove用来设置文件列表移除文件时的钩子函数

    属性on-success用来设置文件上传成功时的钩子函数

    属性on-error用来设置文件上传失败时的钩子函数

    属性on-progress用来设置文件上传时的钩子函数

    属性on-change用来设置文件状态改变时的钩子函数

    属性before-upload用来设置上传文件之前的钩子函数

    方法clearFiles用来清空已上传的文件列表

#### 2.11 评分
用于评分的组件。

* `el-rate`，评分

    属性max用来设置最大分值，默认为5

    属性disabled用来设置是否为只读，默认为false

    属性allow-half用来设置是否允许半选，默认为false

    属性colors用来设置图标的颜色数组，共三个元素

    属性void-color用来设置未选中图标的颜色

    属性show-text用来设置是否显示辅助文字，默认为false

    属性text-color用来设置辅助文字的颜色

    属性texts用来设置辅助文字数组

    事件change在分值改变时触发，参数为改变后的分值

#### 2.12 表单
由输入框、下拉框、单选框、多选框等控件组成，用以收集、校验、提交数据。

* `el-form`，表单

    属性model用来指定表单数据对象

    属性rules用来设置表单验证规则

    属性inline设置表单为行内模式，默认为false

    属性label-position用来设置表单域标签的位置，值可以为left/right/top，默认为right

    属性label-width用来设置表单域标签的宽度

    属性show-message用来设置是否显示校验错误信息，默认为true

    方法validate对整个表单进行校验

    方法validateField对部分表单字段进行校验

    方法resetFields对整个表单进行重置，并移除校验结果

* `el-form-item`，表单项

    属性prop用来设置表单域的model字段

    属性label用来设置标签文本

    属性label-width用来设置表单域标签的宽度

    属性rules用来设置表单验证规则，并将表单项的属性prop设置为需要校验的字段

    属性required用来设置是否必填，如不设置，则会根据校验规则自动生成，默认为false

    属性show-message用来设置是否显示校验错误信息

    属性error用来设置表单域校验错误信息

### 3、数据组件

#### 3.1 表格
用于展示多条结构类似的数据，可对数据进行排序、筛选、对比或其他自定义操作。

* `el-table`，数据表格

    属性data用来设置显示的数据，必须是数组类型

    属性stripe用来设置是否为斑马纹，默认为false

    属性border用来设置纵向边框，默认为false

    属性height用来固定表头

    属性highlight-current-row用来设置是否高亮当前行

    属性default-sort用来设置默认的排序列和排序顺序

* `el-table-column`，表格项

    属性label用来设置列名

    属性width用来设置列的宽度

    属性prop用来设置列的内容

    属性type用来设置列的类型，值可以为selection/index/expand，分别表示多选框/索引/可扩展按钮

    属性fixed用来固定列，值可以为true/left/right，true表示固定在左侧
    
    属性sortable用来设置是否可以排序

    属性align用来设置列的对齐方式，值可以为left/center/right，默认为left

#### 3.2 标签
用于标记和选择。

* `el-tag`，标签

    属性type用来设置标签类型，值可以为primary/gray/success/warning/danger

    属性closable用来设置是否可以关闭标签，默认为false

    属性hit用来设置是否有边框，默认为false

    属性color用来设置标签的背景色

#### 3.3 进度条
用于展示操作进度，告知用户当前状态和预期。

* `el-progress`，进度条

    属性percentage用来设置百分比，必选，默认为0

    属性type用来设置进度条类型，值可以为line/circle，默认为line
    
    属性stroke-width用来设置进度条的宽度，单位为px

    属性text-inside用来设置是否将显示文字内置在进度条内，只在type为line时有效，默认为false

    属性status用来设置进度条当前状态，值可以为success/exception

    属性width用来设置环形进度条的画布宽度，只在type为circle时有效

    属性show-text用来设置是否显示进度条文字内容，默认为true

#### 3.4 树形
用清晰的层级结构展示信息，可展开或折叠。

* `el-tree`，树形

    属性data用来设置显示的数据，必须是数组类型

    属性empty-text用来设置数据为空时显示的文本

    属性props用来设置配置选项，其中label指定节点标签为节点对象的属性值，children指定子树为节点对象的属性值

    属性show-checkbox用来设置是否可以选择节点，默认为false

    属性indent用来设置相邻级节点的缩进，默认为16个像素

    属性node-key用来设置每个树节点的唯一标识

    属性default-expand-all用来设置是否展开所有节点，默认为false

    属性highlight-current用来设置是否高亮当前选中节点，默认为false

    属性current-node-key用来设置当前选中节点的key

    属性accordion用来设置是否每次只打开一个同级节点，默认为false

#### 3.5 分页
当数据量过多时，使用分页分解数据。

* `el-pagination`，分页

    属性layout用来设置组件布局，子组件用逗号分隔，值可以为sizes/prev/pager/next/jumper/->/total/slot

    属性page-size用来设置每页显示的条目数量，默认为10

    属性total用来设置条目的总数

    属性current-page用来设置当前页数，默认为1

    事件size-change在pageSize发生改变时触发，参数为每页条数

    事件current-change在currentPage发生改变时触发，参数为当前页
    
* layout值

    prev表示上一页

    next表示下一页

    pager表示页码列表

    jumper表示跳页元素

    total表示页码总数

    sizes用来设置每页的页码数量

#### 3.6 标记
出现在按钮、图标旁的数字或状态标记。

* `el-badge`，展示新消息数量

    属性value用来设置显示值，可以为数字或字符串

    属性max用来设置显示的最大值，超过最大值时，会显示“+”

    属性hidden用来设置是否隐藏标记，默认为false

    属性is-dot用来设置是否使用圆点代替数量，默认为false

### 4、通知组件

#### 4.1 警告
用于页面中展示重要的提示信息。

* `el-alert`，提示信息，不会自动消失

    属性title用来设置标题，必选

    属性type用来设置主题，值可以为info/success/warning/error，默认为info

    属性description用来设置辅助文本

    属性closable用来设置是否可以关闭，默认为true

    属性show-icon用来设置是否使用图标，默认为false

    事件close在关闭警告时触发

#### 4.2 加载
加载数据时显示动效。可以通过指令或服务实现加载。

* 指令

绑定指令v-loading到一个Boolean值来设置是否显示加载动画。使用body修饰符可以使遮罩插入到DOM的body上。

当需要全屏遮罩时，可以使用fullscreen修饰符。如果需要锁定屏幕的滚动，可以使用lock修饰符。

在绑定v-loading指令的元素上设置element-loading-text属性，在加载图标下方显示文本。

* 服务

引入加载服务：

    import { Loading } from 'element-ui';

需要调用时：

    Loading.service(options);

如果完整引入ElementUI，在Vue上会有一个全局方法$loading，调用时：

    this.$loading(options)

调用会返回一个Loading实例，可以用来关闭加载：

    loadingInstance.close();

参数options为Loading的配置项：

    target用来设置加载需要覆盖的DOM节点
    
    body同v-loading指令中的body修饰符

    fullscreen同v-loading指令中的fullscreen修饰符

    lock同v-loading指令中的lock修饰符

    text用来设置显示加载图标下的文字

#### 4.3 通知
悬浮出现在页面右上角，显示全局的通知提醒消息。

Notification组件提供通知功能，Element注册了$notify方法，接收一个options字面量参数。

* title字段，用来设置通知的标题
* message字段，用来设置通知的正文
* duration字段，用来设置关闭的时间间隔，如果设置为0，则不会自动关闭
* type字段，用来设置通知类型，值可以为info/success/warning/error

也可以不使用type字段，而直接调用$notify的相应方法info/success/warning/error：

    showNotice: function () {
      this.$notify({
        title: '通知',
        message: '这是一个指定类型字段的通知',
        type: 'success',
        duration: 0
      })
    }

#### 4.4 消息提示
常用于主动操作后的反馈提示，与通知的区别是后者更多用于系统级通知的被动提醒。

Element注册了一个$message方法，可以接收一个字符串作为参数，它会被显示为正文内容。如果不设置参数，可以通过message字段来设置消息文字。其他配置选项与通知基本相同。

#### 4.5 消息框
模拟系统的消息提示框而实现的一套模态对话框组件，用于消息提示、成功提示、错误提示、询问信息等。

Element注册了四个方法，分别为$msgbox, $alert, $confirm和$prompt。窗口关闭后，默认会返回一个Promise对象以便进行后续操作的处理。

* title字段，用来设置MessageBox的标题
* message字段，用来设置MessageBox的正文
* type字段，用来设置消息类型，用于显示图标，值可以为info/success/warning/error
* lockScroll字段，用来设置是否锁定滚动，默认为true
* showCancelButton字段，用来设置是否显示取消按钮，如果是confirm和prompt方式调用时，默认为true，否则默认为false
* showConfirmButton字段，用来设置是否显示确认按钮，默认为true
* cancelButtonText字段，用来设置取消按钮的文本内容
* confirmButtonText字段，用来设置确认按钮的文本内容
* closeOnClickModal字段，用来设置是否可以通过点击遮罩关闭消息框，如果是alert方法调用时，默认为false，否则默认为false
* closeOnPressEscape字段，用来设置是否可以通过Esc关闭消息框，如果是alert方法调用时，默认为false，否则默认为false
* showInput字段，用来设置是否显示输入框，如果是prompt方式调用时，默认为true，否则默认为false
* inputValue字段，用来设置输入框的初始文本内容
* inputPattern字段，用来设置输入框的校验表达式
* inputErrorMessage字段，用来设置校验未通过时的提示文本
* callback字段，如果不使用Promise，可以用来指定MessageBox关闭后的回调。有两个参数，action的值为'confirm'或'cancel'；instance为MessageBox实例，可以通过它访问实例上的属性和方法
* beforeClose字段，MessageBox关闭前的回调，会暂停实例的关闭。有三个参数，action的值为'confirm'或'cancel'；instance为MessageBox实例，可以通过它访问实例上的属性和方法；done用于关闭MessageBox实例

### 5、导航组件

#### 5.1 导航菜单
为网站提供导航功能的菜单。导航菜单元素主要有：

* `el-menu`，导航菜单

    属性mode可以设置菜单排列模式，默认为垂直模式

    属性default-active用来设置当前激活菜单的index值

    属性router用来设置是否使用vue-router模式，启用该模式时会以所激活菜单项的index值作为path进行路由跳转

    事件select在菜单激活时触发，第一个参数是菜单项的index，第二个参数是菜单项的path

* `el-submenu`，导航的子菜单

    属性index用来设置子菜单的唯一标识

* `el-menu-item`，导航的菜单项

    属性index用来设置菜单项的唯一标识

    属性route用来指定路由对象

* `el-menu-item-group`，菜单项分组

    属性title用来设置分组名

#### 5.2 标签页
分隔内容上有关联但属于不同类别的数据集合。

* `el-tabs`，标签页

    属性type用来设置标签页类型，值可以为card/border-card

    属性closable用来设置标签是否可以关闭，默认为false

    属性addable用来设置标签是否可以添加，默认为false

    属性editable用来设置标签是否既可以添加也可以关闭，默认为false

    属性value用来绑定选中选项卡的name

    事件tab-click在标签被选中时触发，参数为被选中的标签实例

* `el-tab-pane`，选项卡

    属性label用来设置选项卡的标题

    属性name用来设置选项卡别名，默认为选项卡在选项卡列表中的顺序值

    属性disabled用来设置是否禁用，默认为false

    属性closable用来设置标签是否可以关闭，默认为false

#### 5.3 面包屑
显示当前页面的路径，快速返回之前的任意页面。

* `el-breadcrumb`，面包屑

    属性separator用来设置分隔符，默认为“/”

* `el-breadcrumb-item`，面包屑选项

    属性to用来设置路由跳转对象

    属性replace用来设置在使用to进行路由跳转时，是否向history添加记录，启用则不会添加，默认为false

#### 5.4 下拉菜单
将动作或菜单折叠到下拉菜单中。

* `el-dropdown`，下拉菜单

    属性split-button用来设置是否将下拉触发元素呈现为按钮组，默认为false

    属性type用来设置菜单按钮类型，同按钮组件，只在split-button为true时有效

    属性size用来设置菜单按钮尺寸，同按钮组件，只在split-button为true时有效

    属性menu-align用来设置菜单水平对齐方式，值可以为start/end，默认为end

    属性trigger用来设置触发下拉的行为，值可以为hover/click，默认为hover

    属性hide-on-click用来设置是否在点击菜单项后隐藏菜单，默认为true

    事件command在点击菜单项时触发，参数为菜单项的指令

* `el-dropdown-menu`，下拉菜单项

    属性command用来设置菜单项指令

    属性disabled用来设置是否禁用菜单项，默认为false

    属性divided用来设置是否显示分割线，默认为false

#### 5.5 步骤
引导用户按照流程完成任务的分步导航条，可根据实际应用场景设定步骤，步骤不得少于 2 步。

* `el-steps`

    属性space用来设置每个step之间的间距

    属性direction用来设置显示方向，值可以为vertical/horizontal，默认为horizontal

    属性active用来设置当前激活步骤，默认为0

    属性process-status用来设置当前步骤的状态，值可以为wait/process/finish/error/success，默认为process

    属性finish-status用来设置结束步骤的状态，值可以为wait/process/finish/error/success，默认为finish

    属性align-center用来设置是否将标题描述居中对齐，默认为false

    属性center用来设置是否将组件居中显示，默认为false

* `el-step`

    属性title用来设置步骤的标题

    属性description用来设置步骤的描述

    属性icon用来设置步骤的图标

### 6、其他组件

#### 6.1 对话框

* `el-dialog`，对话框

    属性title用来设置对话框的标题

    属性size用来设置对话框的尺寸，值可以为tiny/small/large/full，默认为small

    属性modal用来设置是否需要遮罩，默认为true

    属性lock-scroll用来设置是否锁定滚动，默认为true

    属性close-on-click-modal用来设置是否可以通过点击modal关闭对话框，默认为true

    属性close-on-press-escape用来设置是否可以通过按下Esc关闭对话框，默认为true

    属性show-close用来设置是否显示关闭按钮，默认为true

    方法open用来打开当前实例

    方法close用来关闭当前实例

#### 6.2 文字提示
常用于展示鼠标 hover 时的提示信息。

* `el-tooltip`，文字提示

    属性effect用来设置默认主题，值可以为dark/light，默认为dark
    
    属性content用来设置提示显示的内容

    属性placement用来设置提示出现的位置，值可以为top/top-start/top-end/bottom/bottom-start/bottom-end/left/left-start/left-end/right/right-start/right-end，默认为bottom

    属性value用来设置状态是否可见，默认为false

    属性disabled用来设置提示是否禁用，默认为false

    属性offset用来设置位置的偏移，默认为0

    属性transition用来设置渐变动画，默认为fade-in-linear

    属性visible-arrow用来设置是否显示箭头，默认为true

    属性openDealy用来设置延迟，单位为毫秒，默认为0

#### 6.3 弹出框
与文字提示基本类似。

* `el-popover`，弹出框

    属性trigger用来设置触发方式，值可以为click/focus/hover/manual，默认为click

    属性title用来设置弹出框标题

    属性content用来设置弹出框显示的内容

    属性width用来设置弹出框的宽度

    属性placement用来设置弹出框出现的位置

    属性value用来设置状态是否可见，默认为false

    属性disabled用来设置弹出框是否禁用，默认为false

    属性offset用来设置位置的偏移，默认为0

    属性transition用来设置渐变动画，默认为fade-in-linear

    属性visible-arrow用来设置是否显示箭头，默认为true

    事件show在显示时触发

    事件hide在隐藏时触发

#### 6.4 跑马灯
在有限空间内，循环播放同一类型的图片、文字等内容。

* `el-carousel`，跑马灯

    属性height用来设置高度，默认为300px

    属性initial-index用来设置初始激活的索引，默认为0

    属性trigger用来设置指示器的触发方式，值可以为click

    属性autoplay用来设置是否自动切换，默认为true

    属性interval用来设置自动切换的时间间隔，单位为毫秒，默认为3000

    属性indicator-position用来设置指示器的位置，值可以为outside/none

    属性arrow用来设置切换箭头，值可以为always/hover/never，默认为hover

    属性type用来设置跑马灯的类型，值可以为card

    事件change在幻灯片切换时触发，参数为当前索引和原索引

    方法setActiveItem用来手动切换幻灯片

    方法prev用来切换至上一张幻灯片

    方法next用来切换至下一张幻灯片

* `el-carousel-item`，跑马灯条目

    属性name用来设置幻灯片的名称，可用作setActiveItem的参数

#### 6.5 手风琴
通过折叠面板收纳内容区域。

* `el-collapse`，手风琴

    属性accordion用来设置是否采用手风琴模式，默认为false

    属性value用来设置当前激活的面板，如果是手风琴模式，绑定值类型需要为String，否则为Array

    事件change在当前激活面板发生改变时触发

* `el-collapse-item`，手风琴条目

    属性name用来设置唯一标识

    属性title用来设置面板标题
