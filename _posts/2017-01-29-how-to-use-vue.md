---
layout: post
title:  "使用Webpack构建Vue2应用"
categories: jekyll update
---

![](https://github.com/gefenghua/MarkdownPictures/raw/master/vue_icon.png)

## 一、概述
> Vue is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is very easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.

Vue.js是一款开源的前端MVVM框架，以数据驱动和组件化的思想构建而成，核心库只关注视图层。兼具很多优秀前端框架的优点，同时又保持简单易用。结合现代工具和支持的类库，Vue可以完美地实现复杂的单页面应用。

[官网地址](http://vuejs.org/)

## 二、特性

### 1、MVVM
MVVM就是Model-View-ViewModel的缩写。ViewModel是Vue.js的核心，它是一个Vue实例。ViewModel位于View层（DOM）与Model层（JS对象）之间。通过DOM Listener监听View层的DOM元素，当DOM元素发生改变时，则改变Model层的数据。当Model层的数据发生改变时，通过Data Bindings来更新View层的DOM元素。这样便实现了双向数据绑定，也是模型驱动的原理。

### 2、指令
由框架提供的特殊属性，以v-为前缀。将指令绑定在DOM元素上，会为元素添加特殊的行为。

### 3、声明式渲染
框架核心是一个可以使用模板语法声明式地将数据渲染到DOM的系统。Vue实例中定义的属性与DOM实现了绑定，并且元素是响应式的。不仅可以直接绑定文本内容，还可以通过指令v-bind绑定DOM元素的属性。

### 4、事件监听
使用v-on指令，绑定监听事件与Vue实例中定义的方法。

### 5、用户输入
使用v-model指令，将表单输入与Vue实例的属性绑定，实现双向数据绑定。

### 6、组件化
这是Vue框架的核心，可以扩展HTML元素，封装可重用的代码。将每个组件的样式、模板、脚本放到一个.vue文件中。每个.vue文件就是一个组件，并且还包含组件之间的依赖关系。可以用独立、可复用的小组件来构建大型应用。

## 三、主要概念及用法

### 1、Vue实例
使用Vue构造函数来创建一个Vue实例，并且传入一个选项对象，选项对象包含数据、模板、挂载元素、方法、生命周期钩子等选项。

	var dataObj = { a: 1};
	var vm = new Vue({
	  el: '#element_id',
	  data: dataObj,
	  created: function () {
	    console.log('Data is: ' + data.a);
	  }
	});

`el`属性指向View，#element_id表示该实例将挂载到id为`element_id`的元素下。`data`属性指向Model，表示该实例的数据对象。`created`属性表示当实例创建完成之后将调用该方法。

每个Vue实例都会代理其data对象里所有的属性。只有被代理的属性才是响应式的，如果在实例创建之后添加新的属性，不会触发视图更新。

	vm.a === dataObj.a; // true

除了data对象里的属性，Vue实例还包含一些实例属性与方法，以`$`为前缀，以便与其代理的data对象里的属性区分开来。

	vm.$data === dataObj; // true
	vm.$el === document.getElementById('element_id'); // true

每个Vue实例在被创建时都会经过一系列的初始化过程，需要配置数据观察、编译模板、挂载实例到DOM、数据变化时更新DOM等。与此同时，也会激活一些生命周期钩子函数，可以用来执行自定义的逻辑。所有生命周期钩子函数的this指向调用它的Vue实例。Vue框架中没有控制器的概念，所有自定义逻辑都是分布在这些生命周期钩子函数中。

### 2、模板语法
Vue框架使用基于HTML的模板语法，允许声明式地将渲染的DOM与底层的实例数据绑定起来。Vue将模板编译成虚拟DOM渲染函数，并且能够计算出重新渲染组件所需花费的最小代价。

使用双大括号直接插入文本内容：

	<span>{{ msg }}</span>

双大括号标签会被对应数据对象的属性值所代替，并且会随着数据对象属性值的变化而改变。

使用`v-html`指令插入HTML内容：

	<div v-html="rawHtml"></div>

双大括号会将数据解释为纯文本而不是HTML，要想输出真正的HTML，就需要使用v-html指令。由于插入的内容是HTML，所以会忽略其中的数据绑定。

注意：不能使用v-html来组合局部模板，因为Vue不是基于字符串的模板引擎。应该使用组件来进行UI的复用和组合。

使用`v-bind`指令绑定HTML属性：

	<div v-bind:id="dynamicId"></div>

双大括号不能在HTML的属性中使用，要想在HTML的属性中使用数据对象的属性，应该使用v-bind指令。除了用来直接绑定属性值，该指令还支持JavaScript表达式。

	<div v-bind:id="'list-' + id"></div>

注意：每个绑定只能包含单个表达式。

常用的指令包括：

* v-if：条件渲染指令，根据表达式值的真假来插入或删除元素
* v-show：也是条件渲染指令，主要用来设置样式，与v-if的区别在于使用v-show指令的元素始终会被渲染到HTML
* v-else：该指令必须紧跟在v-if或v-show指令的后面配合使用
* v-for：遍历一个数组，将其渲染成一个列表
* v-bind：绑定指令，可以带一个参数，用冒号分隔，该参数通常是HTML元素的属性
* v-on：事件监听指令，用法与v-bind类似，参数通常是事件名称

注意：为了简化常用的指令，v-bind可以缩写为一个冒号`:`，v-on指令可以缩写为`@`符号。

除了指令之外，Vue框架还允许定义过滤器，对文本内容进行格式化。过滤器应该被添加在双大括号或v-bind表达式的尾部，使用管道符进行分隔。

	{{ msg | capitalize }}

	<div v-bind:id="rawId | formatId"></div>

过滤器函数总是接收表达式的值作为第一个参数。

	filters: {
	  capitalize: function (value) { ... }
	}

### 3、计算属性
在模板中使用表达式虽然便利，但不适合用于复杂的操作，在模板中放入过多的逻辑会很难维护。计算属性依赖于数据模型，当数据模型发生变化时，计算属性也会随之改变。

	<div id="example">
	  <p>Original message: "{{ msg }}"</p>
	  <p>Computed reversed message: "{{ reversedMsg }}"</p>
	</div>

	var vm = new Vue({
	  el: '#example',
	  data: {
	    msg: 'hello'
	  },
	  computed: {
	    reversedMsg: function () {
	      return this.msg.split('').reverse().join('');
	    }
	  }
	});

computed与methods的比较：

计算属性基于它的依赖进行缓存，只有当它的相关依赖发生变化时才会重新取值，否则会立刻返回之前的计算结果。通过在methods中定义函数虽然能够产生同样的结果，但是函数在每次渲染时都会被执行，无论结果是否发生变化。

computed与watch的比较：

Vue提供了一个方法$watch，它用于观察实例上的数据变化。虽然也能够实现对数据变化做出相应改变的操作，但在某些情况下不如计算属性简便（如：某属性依赖于多个其他属性，使用watch方法的话需要对每个依赖属性进行观察并计算处理）。当响应数据变化时，如果需要执行异步操作或花销较大的操作，可以使用watch方法。

### 4、class和style绑定
数据绑定的常见需求是操作元素的class列表和内联style，v-bind指令用于class和style时，表达式结果除了字符串，还可以是对象或数组。

可以传给v-bind:class一个对象，以动态地切换class属性。也可以在对象中传入多个属性，并且可以与普通的class属性共存。最佳实践是绑定返回对象的计算属性，在计算属性的方法中实现逻辑。

	<div v-bind:class="{ active: isActive }"></div>

可以传给v-bind:class一个数组，以应用一个class列表。如果有多个条件class，可以在数组语法中使用对象语法。当在一个组件上使用class属性时，这些class会被添加到根元素上，已经存在的class不会被覆盖。

	<div v-bind:class="[{ active: isActive }, errorClass]"></div>

v-bind:style的对象语法与CSS十分相似，实际上是一个JavaScript对象。通常情况下会绑定到一个style对象，也可以结合计算属性使用。v-bind:style的数组语法可以将多个样式对象应用到一个元素上。当使用特定前缀的CSS属性时，Vue会自动侦测并添加相应的前缀。

	<div v-bind:style="styleObj"></div>

	data: {
	  styleObj: {
	    color: 'red',
	    fontSize: '12px'
	  }
	}

### 5、条件渲染
使用指令v-if、v-else-if、v-else可以进行条件判断，根据返回值的真假来决定是否进行渲染。v-else或v-else-if的元素必须紧跟在v-if或v-else-if的元素后面，否则不能被识别。

如果想一次性切换多个元素，可以用\<template>元素将其封装，然后在template元素上应用v-if指令。v-show与v-if的用法基本相同，但是v-show指令不支持\<template>的用法。v-show只是简单地切换元素的CSS属性display，元素会始终渲染并保持在DOM上。

### 6、列表渲染
使用v-for指令根据数组的选项列表进行渲染，需要使用`item in items`的语法，其中items是数组，item是数组元素的别名。还可以使用`(item, index) in items`的语法，将可选参数index作为当前项的索引。可以使用of来替代in作为分隔符。如同v-if的模板，可以使用带有v-for的\<template>标签来渲染多个元素。

	<ul id="example">
	  <li v-for="item in items">
	    {{ item.msg }}
	  </li>
	</ul>

	el: '#example',
	data: {
	  items: [
	    { msg: '1' },
	    { msg: '2' }
	  ]
	}

在v-for块中，可以访问父作用域的属性。

	<ul id="child">
	  <li v-for="item in items">
	    {{ parentMessage }} + {{ item.msg }}
	  </li>
	</ul>

	el: '#child',
	data: {
	  parentMessage: 'Parent',
	  items: [
	    { msg: '1' },
	    { msg: '2' }
	  ]
	}

除了使用数组，还可以通过对象的属性来迭代，并且支持第二个参数作为键名，第三个参数作为索引。

	<div v-for="(value, key, index) in object">
	  {{ index }}. {{ key }} : {{ value }}
	</div>

	el: '#example',
	data: {
	  object: {
	    firstName: 'sean',
	    lastName: 'ge'
	  }
	}

可以指定迭代次数，重复渲染模板。

	<span v-for="n in 10">{{ n }}</span>

Vue包含一组观察数组的变异方法，这些方法会改变所调用的原始数组，并触发视图更新。这些方法包括：push()、pop()、shift()、unshift()、splice()、sort()、reverse()等。还有一些非变异方法，不会改变原始数组，但是会返回一个新的数组。这些方法包括：filter()、concat()、slice()等。

由于JavaScript的限制，Vue不能检测到利用索引设置某一项或修改数组的长度而导致变动的数组，需要通过调用数组的splice方法来处理上述情况。

### 7、事件处理器
使用v-on指令可以监听DOM元素上的事件。事件处理可以通过两种方式：方法名和内联语句。

直接使用方法名：

	<button v-on:click="greet">Greet</button>

使用内联语句：

	<button v-on:click="say('Hello')">Hello</button>

如果需要在内联语句中访问原生的DOM事件，可以将特殊变量$event传入方法。

	<button v-on:click="warn('Form cannot be submitted yet.', $event)">Submit</button>

	methods: {
	  warn: function (msg, event) {
	    if (event) event.preventDefault();
	    alert(msg);
	  }
	}

虽然可以在methods中调用原生事件，但更好的实现方式是methods只包含数据逻辑，而不处理DOM事件细节。Vue提供了多个事件修饰符来解决这个问题，修饰符包括：.stop、.prevent、.capture、.self、.once等。在监听键盘事件时，允许为v-on添加按键修饰符。

	<!-- 阻止单击事件冒泡 -->
	<a v-on:click.stop="doThis"></a>
	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>
	<!-- 修饰符可以串联 -->
	<a v-on:click.stop.prevent="doThat"></a>
	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>
	<!-- 添加事件侦听器时使用事件捕获模式 -->
	<div v-on:click.capture="doThis">...</div>
	<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
	<div v-on:click.self="doThat">...</div>
	<!-- 该事件最多只被触发一次 -->
	<a v-on:click.once="doThis"></a>

### 8、表单控件
使用v-model指令在表单控件元素上创建双向数据绑定，负责监听用户的输入事件并根据控件类型自动选择正确的方法来更新元素。

文本框：

	<input v-model="msg" placeholder="Single line text" />
	<p>Message is: {{ msg }}</p>
	<textarea v-model="ta-msg" placeholder="Multiple lines text" />
	<p>Textarea message is: {{ ta-msg }}</p>

复选框：

	<!-- 单个复选框绑定到一个逻辑值 -->
	<input type="checkbox" id="checkbox" v-model="checked" />
	<label for="checkbox">{{ checked }}</label>

	<!-- 多个复选框绑定到一个数组 -->
	<input type="checkbox" id="one" value="1" v-model="checkedNum" />
	<label for="one">1</label>
	<input type="checkbox" id="two" value="2" v-model="checkedNum" />
	<label for="two">2</label>
	<input type="checkbox" id="three" value="3" v-model="checkedNum" />
	<label for="three">3</label>
	<br>
	<span>Checked number: {{ checkedNum }}</span>

	data: {
	  checkedNum: []
	}

单选按钮：

	<input type="radio" id="one" value="1" v-model="picked" />
	<label for="one">1</label>
	<input type="radio" id="two" value="2" v-model="picked" />
	<label for="two">2</label>
	<br>
	<span>Picked number: {{ picked }}</span>

选择列表：

	<!-- 多选列表绑定到一个数组 -->
	<select v-model="selected" multiple>
	  <option>A</option>
	  <option>B</option>
	  <option>C</option>
	</select>
	<br>
	<span>Selected option: {{ selected }}</span>

如果列表的选项是动态选项，可以使用v-for进行渲染。

	<select v-model="selected">
	  <option v-for="option in options" v-bind:value="option.value">
	    {{ option.text }}
	  </option>
	</select>
	<br>
	<span>Selected option: {{ selected }}</span>

	data: {
	  selected: '',
	  options: [
	    { text: 'One', value: 1},
	    { text: 'Two', value: 2},
	    { text: 'Three', value: 3}
	  ]
	}

对于复选框、单选按钮、选择列表的选项，v-model绑定的value通常是静态字符串。如果想绑定value到实例的一个动态属性上，可以使用v-bind来实现。

默认情况下，v-model在input事件中会同步输入框的值与数据。添加修饰符`.lazy`，就会变成在change事件中进行同步。

默认情况下，输入框返回的值是一个字符串类型。如果想自动将用户输入转换为Number类型，可以添加`.number`修饰符。

如果要自动消除用户输入的首尾空格，可以添加`.trim`修饰符。

## 四、组件
Web组件化是目前开发大型网页或web应用的趋势。Vue框架在组件上的设计十分巧妙，定义了*.vue格式的文件，将每个组件的模板、脚本和样式都整合在一个文件中。这样每个文件就是一个组件，不仅包含了组件的结构、外观、行为特性，还包含了组件之间的依赖关系。

### 1、组件的使用
使用组件之前，首先需要对组件进行注册。注册一个全局组件，可以调用Vue的component方法。自定义的标签名建议遵循W3C规则，即小写并且包含短横杠。

	Vue.component('my-component', {
	  // options
	})

组件在注册之后，就可以在父实例的模块中以自定义元素的形式被使用。要确保在初始化根实例之前注册组件，这样就会用组件的模板替代组件标签进行渲染。

	<div id="example">
	  <my-component></my-component>
	</div>

	Vue.component('my-component', {
	  template: '<div>A customer component!</div>'
	})

	new Vue({
	  el: '#example'
	})

使用组件实例选项注册，可以使组件只能在另一个实例或组件的作用域中可用。这种封装的方式也适用于其他可注册的功能，如指令等。

	var child = {
	  template: '<div>A customer component!</div>'
	}

	new Vue({
	  // ...
	  components: {
	    'my-component': child
	  }
	})

因为Vue只有在浏览器解析之后才能获取模板内容，当使用DOM作为模板时，会受到HTML的一些限制。有些元素限制了能被它包含的元素，如\<option>元素只能出现在其他元素内部。在自定义组件中使用这些受限制的元素，会被认为是无效的内容。如果使用字符串模板，则不会受到这些限制。

	<table>
	  <my-row></my-row>
	</table>

此时可以使用特殊的`is`属性来解决这个问题，将自定义组件当做属性，而不是元素来使用。

	<table>
	  <tr is="my-row"></tr>
	</table>

使用组件时，大多数构造器中的选项可以在注册组件的时候使用。但是data是个例外，在组件中data必须是一个函数。为每个组件返回新的data对象，每个对象有自己的内部状态，以避免多个组件共享同一个对象。

	<div id="example">
	  <simple-counter></simple-counter>
	  <simple-counter></simple-counter>
	</div>

	Vue.component('simple-counter', {
	  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
	  data: function () {
	    return {
	      counter: 0
	    }
	  }
	})

组件之间协同工作通常都是父子关系，即组件A在它的模板中使用组件B。父组件通过props给子组件传递数据，子组件通过events给父组件发送消息。定义良好的接口以便更好地解耦父子组件。

### 2、Prop的使用
组件实例的作用域是孤立的，不能在子组件的模板中直接引用父组件的数据，但可以通过prop将父组件的数据传递给子组件。prop是父组件用来传递数据的一个自定义属性，子组件需要显式地使用props选项声明该prop属性。

	<child msg="hello"></child>

	Vue.component('child', {
	  props: ['msg'],
	  template: '<span>{{ msg }}</span>'
	})

可以使用v-bind动态绑定props的值到父组件的数据上，每当父组件的数据发生变化时，该变化也会传递给子组件。

	<div>
	  <input v-model="parentMsg" />
	  <br>
	  <child v-bind:my-message="parentMsg"></child>
	</div>

	Vue.component('child', {
	  props: ['my-message'],
	  template: '<span>{{ my-message }}</span>'
	})

在模板中，字面量的值是一个字符串，切忌以字面量的值当成数值进行传递。需要使用v-bind指令，让它的值被当做JavaScript表达式进行计算并传递。

	<!-- 传递字符串“1” -->
	<comp some-prop="1"></comp>

	<!-- 传递数值1 -->
	<comp v-bind:some-prop="1"></comp>

prop是单向绑定的，即父组件的属性发生变化时，将传递给子组件，反之则不会。通常有两种改变prop的情况。

prop作为初始值传入，子组件将它作为本地数据的初始值使用：

	props: ['initialCounter'],
	data: function() {
	  return { counter: this.initialCounter }
	}

prop作为需要被转变的原始值传入：

	props: ['size'],
	computed: {
	  normalizedSize: function() {
	    return this.size.trim().toLowerCase()
	  }
	}

注意：由于在JavaScript中对象和数组是引用类型，指向同一个内存空间，如果prop是对象或数组，在子组件内部改变它会影响父组件的状态。

组件可以为props指定校验规则：

	Vue.component('example', {
	  props: {
	    <!-- 基础类型 -->
	    propA: Number,
	    <!-- 多种类型 -->
	    propB: [String, Number],
	    <!-- 字符串类型并且是必需的 -->
	    propC: {
	      type: String,
	      required: true
	    },
	    <!-- 数值类型并且有默认值 -->
	    propD: {
	      type: Number,
	      default: 100
	    },
	    <!-- 对象类型并且默认值通过一个工厂函数返回 -->
	    propE: {
	      type: Object,
	      default: function() {
	        return { message: "hello" }
	      }
	    },
	    <!-- 自定义校验函数 -->
	    propF: {
	      validator: function(value) {
	        return value > 10
	      }
	    }
	  }
	})

### 3、自定义事件
每个Vue实例都实现了事件接口，使用$on(eventName)监听事件，使用$emit(eventName)触发事件。父组件可以在使用子组件的地方直接用v-on来监听子组件触发的事件。如果想在某个组件的根元素上监听一个原生事件，可以使用.native修饰v-on指令。

	<div id="counter-event-example">
	  <p>{{ total }}</p>
	  <button-counter v-on:increment="incrementTotal"></button-counter>
	  <button-counter v-on:increment="incrementTotal"></button-counter>
	</div>

	Vue.component('button-counter', {
	  template: '<button v-on:click="increment">{{ counter }}</button>',
	  data: function() {
	    return { counter: 0 }
	  },
	  methods: {
	    increment: function() {
	      this.counter += 1;
	      this.$emit('increment');
	    }
	  }
	})

	new Vue({
	  el: '#counter-event-example',
	  data: { total: 0 },
	  methods: {
	    incrementTotal: function() {
	      this.total += 1;
	    }
	  }
	})

自定义事件也可以用来创建自定义的表单输入组件。使用v-model进行双向数据绑定，要想让组件的v-model生效，必须接收一个value属性，并且在value改变时触发input事件。

	<custom-input v-bind:value="something" v-on:input="something=arguments[0]"></custom-input>

除了父子组件之间可以通信外，非父子关系的组件之间有时也需要进行通信。在简单的场景下，可以使用一个空的Vue实例作为中央事件总线，由它负责事件的监听和触发。如果场景比较复杂，应该考虑使用专门的状态管理模式。

	var bus = new Vue();

	// 在组件A的methods中触发
	bus.$emit('id-selected', 1)

	// 在组件B的生命周期钩子函数中监听
	bus.$on('id-selected', function (id) {
	  ...
	}

### 4、使用slot分发内容
在使用组件时，常常要进行组件的组合。为了让组件可以组合，需要一种方式来混合父组件的内容与子组件的模板，这个过程被称为内容分发。

父组件模板的内容在父组件作用域内编译，子组件模板的内容在子组件作用域内编译。

	<child-component>
	  {{ msg }}
	</child-component>

这里的msg应该绑定到父组件的数据，它的作用域属于父组件。

常见的错误是在父组件模板内将指令绑定到子组件的属性或方法上。

	<child-component v-show="childProperty"></child-component>

此时的语句是无效的，因为父组件模板不应该知道子组件的状态。

如果要绑定子组件内的指令到一个组件的根节点，应该在子组件的模板内进行处理。

	Vue.component('child-component', {
	  template: '<div v-show="childProperty">Child</div>',
	  data: function() {
	    return { childProperty: true }
	  }
	}

所以，分发内容是在父组件的作用域内进行编译。

Vue实现了一个内容分发的API，使用特殊的\<slot>元素作为原始内容的插槽。除非子组件模板包含至少一个\<slot>插槽，否则父组件的内容将会被丢弃。当子组件模板只有一个没有属性的slot时，父组件整个内容片段将插入到slot所在的DOM位置。最初在\<slot>标签中的内容都被视为备用内容。备用内容在子组件的作用域内编译，只有在宿主元素为空，并且没有要插入的内容时才显示。

子组件模板：

	<div>
	  <h2>子组件标题</h2>
	  <slot>备用内容</slot>
	</div>

父组件模板：

	<div>
	  <h1>父组件标题</h1>
	  <child-component>
	    <p>父组件内容</p>
	  </child-component>
	</div>

渲染结果：

	<div>
	  <h1>父组件标题</h1>
	  <div>
	    <h2>子组件标题</h2>
	    <p>父组件内容</p>
	  </div>
	</div>

\<slot>元素可以使用一个特殊的属性name来配置如何分发内容。具名slot将匹配内容片段中有对应slot属性的元素。可以有一个匿名slot，作为默认的内容片段。如果没有默认的slot，找不到匹配的内容片段将会被丢弃。

子组件模板：

	<div class="container">
	  <header>
	    <slot name="header"></slot>
	  </header>
	  <main>
	    <slot></slot>
	  </main>
	  <footer>
	    <slot name="footer"></slot>
	  </footer>
	</div>

父组件模板：

	<div>
	  <h1 slot="header">Header</h1>
	  <p>父组件内容</p>
	  <p slot="footer">Footer</p>
	</div>

渲染结果：

	<div class="container">
	  <header>
	    <h1>Header</h1>
	  </header>
	  <main>
	    <p>父组件内容</p>
	  </main>
	  <footer>
	    <p>Footer</p>
	  </footer>
	</div>

作用域插槽是一种特殊类型的插槽，用来使用一个可重用模板替换已渲染元素。在子组件中，只需将数据传递到插槽。在父组件中，特殊属性scope的\<template>元素，表示它是作用域插槽的模板。scope的值对应一个变量名，该变量接收从子组件中传递的prop对象。

子组件模板：

	<div class="child">
	  <slot text="hello from child"></slot>
	</div>

父组件模板：

	<div class="parent">
	  <child>
	    <template scope="props">
	      <span>hello from parent</span>
	      <span>{{ props.text }}</span>
	    </template>
	  </child>
	</div>

渲染结果：

	<div class="parent">
	  <div class="child">
	    <span>hello from parent</span>
	    <span>hello from child</span>
	  </div>
	</div>

作用域插槽更具代表性的用例是列表组件，允许组件自定义如何渲染列表每一项。

列表组件模板：

	<ul>
	  <slot name="item" v-for="item in items" :text="item.text"></slot>
	</ul>

父组件模板：

	<my-list :items="items">
	  <template slot="item" scope="props">
	    <li class="my-item">{{ props.text }}</li>
	  </template>
	</my-list>

### 5、动态组件
多个组件可以使用同一个挂载点，然后动态地进行切换。使用\<component>元素，动态地绑定到它的is属性上。

	<component v-bind:is="currentView"></component>

	var vm = new Vue({
	  el: '#example',
	  data: { currentView: 'home' },
	  components: {
	    home: {}
	    posts: {}
	    archive: {}
	  }
	})

如果想把切换出去的组件保留在内存中，保留它的状态或避免重新渲染，可以添加`keep-alive`指令。

	<keep-alive>
	  <component v-bind:is="currentView"></component>
	</keep-alive>

### 6、可复用组件
可复用组件应该定义一个清晰的公开接口。组件的API来自三个部分：props，允许外部环境传递数据给组件；events，允许组件在外部环境中触发效果；slots，允许外部环境将额外的内容组合在组件中。

	<my-component :foo="baz" :bar="qux" @eventA="doThis" @eventB="doThat">
	  <img slot="icon" src=" ... " />
	  <p slot="mainText">Hello</p>
	</my-component>

### 7、子组件索引
虽然可以使用props和events，但有时仍然需要直接访问子组件，可以使用ref为子组件指定一个索引ID。当ref和v-for一起使用时，ref是一个数组或对象，包含相应的子组件。$refs只有在组件渲染完成之后才会填充，并且是非响应式的，应当避免在模板或计算属性中使用。

	<div id="parent">
	  <user-profile ref="profile"</user-profile>
	</div>

	var parent = new Vue({ el: '#parent' });
	var child = parent.$refs.profile;

### 8、异步组件
Vue允许将组件定义为一个工厂函数，动态地解析组件的定义。只在组件需要渲染时触发工厂函数，并且把结果缓存起来，用于再次渲染。工厂函数接收一个resolve回调，在收到从服务器下载的组件定义时调用。也可以调用reject(reason)来指示加载失败。

	Vue.component('async-example', function (resolve, reject) {
	  setTimeout(function () {
	    resolve({
	      template: '<div>Async!</div>'
	    })
	  }, 1000)
	})

### 9、递归组件
组件可以在它的模板中递归地调用自己，但是需要有name选项才可以。当注册了一个全局组件时，自动将全局的ID设置为组件的name选项。递归组件可能会导致死循环，所以需要确保递归调用有终止条件。
