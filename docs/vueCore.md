# Vue.js

[TOC]

## 为什么要使用Vue.js

Z：vue.js的优点如下：

- vue.js将前端的设计，从关注DOM转移到关注数据上

- data属性值发生变化时，会自动更新（前提是该属性存在，默认值也行）

  使用``Object.freeze(属性)``可以阻止值被更新

  ```javascript
  var obj = {mess:"不改变的内容"};
  Object.freeze(obj)
  ```

## 语法

### 1.指令

#### v-bind

Z：指令带有``v-``,例如强制绑定v-bind：

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

v-bind:捆绑指令，title指向data中的message变量``v-bind:title="message"``。

标签中style属性的值是一个字符串，而v-bind:style属性的值是对象。如果要将style转化为:style，写法为``:style="{fontSize: '20px',color: itemProps.checked ? 'red':'blue'}"``,属性名改为驼峰式。

M：那如果我要修改属性值呢?

Z：只要执行``app2.message = '新消息';``js语句，message属性将会被修改。   

M：``v-bind:title="变量名"``最后会被渲染为``title="message的变量内容"``，这里title的属性变化相当于attr的作用。

_<a v-bind:href="url">可以缩写为<a :href="url">，还可以绑定src等属性_  

#### v-if

M：我们之前要控制元素是否显示，使用js的判断，控制元素的display属性，而在vue中怎么使用呢？

Z：如下所示：

```html
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
```

```javascript
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

v-if判断变量seen的属性值，如果值为false，数据将消失，否则正常显示。

M：那除了if，有没有else呢？

Z：如下代码

```javascript
	<div v-if="Math.random() > 0.5">
	  Now you see me
	</div>
	<div v-else-if="Math.random() < 0.3">
	  Now you see another me
	</div>
	<div v-else>
	  Now you don't
	</div>
```

M：这里的属性切换是导致元素直接remove/add，并非简单的隐藏。

#### v-show

M：因为有些元素需要频繁显示隐藏，我想在css层面隐藏它，在vue中怎么实现？

Z：如下代码

```html
<div id="demo">
	<div v-show="ok">
	  Now you see me
	</div>
	<div v-show="no">
	  Now you see another me
	</div>
</div>
```

```javascript
var vm = new Vue({
	el: '#demo',
	data: {
	  ok: true,
	  no: false
	}
})
```

#### v-for

M：对于列表的前端显示，我们一般会使用EL表达式的``<c:forEach>``语句，在vue中怎么实现呢？

Z：实现代码如下：

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```javascript
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

在渲染时，添加了v-for的li将会被复制多份，传过来的内容是一个数组

M：如果要往数组中新增内容，怎么实现呢？

Z：使用``app3.todos.push({ text: '新项目' })``即可往数组中追加内容   

M：v-if和v-for组合怎么使用？

Z：如果是for中隐藏不显示的列，可以用for循环**计算属性**返回的值，如下代码

```javascript
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {   //filter方法
      return user.isActive
    })
  }
}
```

_尽量避免将for和if写在同一个元素上面``<div v-for="grocery in groceryList" v-if="grocery.isShow">，这样可以使for操作和if操作解耦``_    

如果是通过if判断，是否显示整块列，如下代码：

```html
	<div v-if="flag">
		<div v-for="grocery in groceryList" >
		  <a>{{grocery.text}}</a>
		</div>
	</div>
```

M：如果我有一个对象，但是不知道它的key是什么，要用什么方式把它遍历出来呢？

Z：可以用v-for循环key，value进行显示

```html
<ul id="v-for-object" class="demo">
	<div v-for="(value, key) in object">
	  {{ key }}: {{ value }}
	</div>
</ul>
```

```javascript
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```

如果只要value，使用以下代码即可

```HTML
	<div v-for="value in object">
	  {{ value }}
	</div>
```

_在2.2.0+版本中，v-for时key是必须添加的。_

#### v-on   

M：如果我们要做按钮，一般就是添加onclik属性，然后在js中添加该方式，在vue中怎么实现呢？

Z：如下代码，绑定事件监听：

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

用v-on监听click事件，指定方法名。将方法添加到vue实例的methods属性中

_<a v-on:click="test">可以缩写为<a @click="test">_  

#### v-model

M：我想实现显示栏实时显示输入框的内容，一般用到onkeyup监听键盘，每次都执行attr方法替换显示的标签内容。怎么vue中怎么实现呢？

Z：代码如下：

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```javascript
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

vue通过v-model方法双向绑定vue变量&输入框，vue变量值变化，显示内容则实时更新。

M：能不能对checkbox的状态实时显示呢？

Z：如下代码：

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

直接遍历checkbox就可以获取到checkbox的状态了   

M：那同样是复选框，我要获取多选的值需要怎么做？

Z：代码如下：

```html
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
```

```javascript
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

直接将多个checkbox的标签设置用一个``v-model``属性，在data中对应的checkedNames数组就会获取到选中的value值。

_单选radio的使用方式相似_

M：那下拉框怎么实时获取？

Z：将``v-model``添加在select上

```html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```javascript
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

M：下拉多选怎么实现？

Z：代码如下，添加``multiple``即可：

```vue
<div id="example-5">
  <select v-model="selected" multiple>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>

<script>
new Vue({
  el: '#example-5',
  data: {
    selected: ''
  }
})
</script>
```

M：有时候输入框的内容不想让别人输入特殊字符，需要怎么限定呢？

Z：当使用了``<input v-model.number="age" type="number">``会对非浮点数之外的内容进行限制。   

M：如果该模块是动态生成的，v-model重复怎么办？

Z：在同一个模块下，v-model有自己的作用域，不会跟其他的v-model产生干扰

#### v-html

M：当我们使用``<p>{{msg}}</p>``的时候，msg文本会直接显示在p标签里；而如果msg是一段HTML代码，我们希望它以html形式直接插入到p标签中怎么实现呢？

Z：需要用到v-html属性，代码如下：

```html
<p v-html="msg"></p>
```

#### key属性

M：key属性有模式作用呢，为什么要添加key属性将标签区分开来？

Z：代码如下：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

在这段代码中，label只被渲染了一次，而用到了两个地方（第二次只是渲染了文本内容）。

而添加了key的input将会被重新渲染，如果不重新渲染，输入在Username中的内容将会保存到Email文本框中。

#### 自定义指令  

M：如果我经常要转化大小写，希望自己封装指令，怎么实现呢？

Z：指令分两种，局部指令 & 全局指令，都用``directive处理函数``定义指令：

```javascript
  //全局指令
  Vue.directive('upper-text', function (el, binding) {
    console.log(el, binding)
    el.textContent = binding.value.toUpperCase()  //el是标签，binding包含标签信息
  })
  //局部指令
    new Vue({
        el: '#test',
        data: {
            msg: "I Like You"
        },
        // 注册局部指令
        directives: {
            'lower-text'(el, binding) {
                console.log(el, binding)
                el.textContent = binding.value.toLowerCase()
            }
        }
  })
```

使用指令

```html
<div id="test">
  <p v-upper-text="msg"></p>
  <p v-lower-text="msg"></p>
</div>

<div id="test2">
  <p v-upper-text="msg"></p>
  <p v-lower-text="msg"></p>
</div>
```





### 2.钩子

Z：实例的创建也存在生命周期，我们可以在不同的生命周期执行不同的代码，叫做生命周期钩子函数。常见的钩子函数有：如 mounted、updated和 destroyed

#### created

M：vue中created钩子函数使用方式如下：

```javascript
var vm = new Vue({
  el: '#app-7',
  data: {mess:"内容"};,
  created: function () {
    // `this` 指向 vm 实例
    alert("创建时调用");
  }
})
```

### 3.属性

#### computed计算属性  

M：如果我们要实现对一个值的调整，需要获取那个原值，处理之后再做显示。这样子做不到原值变化的时候生成值同步更新。vue是怎么实现的呢？

Z：代码如下：

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

直接用{{变量名}}获取该变量名reversedMessage下的函数值（这个函数成为getter函数）；该变量为message转化而来，message一变，reversedMessage也将随之改变。

_computed属性 = 方法功能 + 侦听功能 + 缓存功能_  

#### methods方法属性

Z：方法属性的使用跟计算属性基本一致，代码如下：

```html
{{ reversedMessage() }}   <!--方法需要加() -->
```

```javascript
var vm = new Vue({
    el: '#example',
    data: {
        message: 'Hello'
    },
    // 在组件中
    methods: {
        reversedMessage: function () {
            return this.message.split('').reverse().join('')
        }
	}
})
```

M：既然如此，为什么不统一使用methods呢？

Z：主要是使用场景的问题，computed是基于原数据进行计算的，计算完之后会对结果进行缓存；下次调用将不再执行getter函数，直至原数据更新才再次调用。

而methods则是每次都执行一遍，相比之下，computed更加节省资源。

#### watch侦听属性

能用computed的就不要用watch，虽然watch都能实现，但资源消耗更多。

Z：watch除了可以用``vm.$watch('mess', function (newValue, oldValue) {...``来编写实例方法，还可以用以下方式实现：

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

对firstName和lastName分别进行侦听；因为这个侦听属性也是基于原数据进行处理，所以也可以用computed计算属性代替

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {    <!-- 此处定义fullName -->
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

M：fullName是computed下定义的变量，自带getter方法，所以data中无需再定义fullName。问题是如果我要改变fullName的值需要怎么做呢？

Z：扩展fullName的setter方法，代码如下：

```javascript
  computed: {
    fullName: {
		get: function () {
			return this.firstName + ' ' + this.lastName
		},
		set: function(newValue){
		  var names = newValue.split(',')
		  this.firstName = names[0]
		  this.lastName = names[names.length - 1];
		}
	}
  },
```

```javascript
vm.fullName = '111111,22222';
```

直接给fullName赋值即可调用set方法

M：如果我想用侦听器侦听输入的内容，然后访问api；但是又不想变化一次就输入一次，怎么设置多次变化的访问频率呢？

Z：分析以下一个demo:

```vue
<script src="https://unpkg.com/vue"></script>

<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
	
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```

这里用到了debounce进行频率的控制，每500ms才会尝试一次访问。访问通过``axios.get('https://yesno.wtf/api')``获取到response后对其内容进行解析显示。

### 4.绑定HTML Class

#### Class绑定

M：如果我想让元素动态地变化属性，一般情况下就是监测改变标签的css样式。在vue中该怎么实现呢？

Z：可以用v-bind绑定class属性，用class绑定css样式；代码如下：

```html
<div v-bind:class="{ active: isActive }"></div>
```

其中isActive是一个布尔值，当isActive值为true时，``class=active``才会被渲染出来。

M：如果我要定义多个类，该怎么实现呢？

Z：通过对的方式传输

```html
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

M：如果我现在添加那个类，需要一定的逻辑判断，又要怎么做呢？

Z：将普通属性转变为计算属性

```html
<div id="demo">
	<div v-bind:class="classObject"></div>
</div>
```

```javascript
var vm = new Vue({
	el: '#demo',
	data: {
	  isActive: true,
	  error: null
	},
	computed: {
	  classObject: function () {
		var flagA = this.isActive && !this.error;
		var flagB = false;
		flagB = flagA;
		return {active:flagA ,'text-danger': flagB}
	  }
	}
})
```

#### CSS内联绑定  

M：如果我想把CSS写成内联样式，在vue中该怎么实现呢？

Z：用``v-bind:style``绑定键值对就可以了，代码如下：

```html
<div id="demo">
	<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">内容</div>
</div>
```

```javascript
var vm = new Vue({
	el: '#demo',
	data: {
	  activeColor: 'red',
	  fontSize: 30
	}
})
```

### 5.元素

#### 数组

M：怎么获取数组脚标呢？

Z：使用v-for的方式``<li v-for="(p,index) in persons" :key="index">``

M：如果我要删除指定数组元素，怎么实现呢？

Z：传入数组脚标，然后使用函数``splice``

```javascript
deleteP(index){
    //删除person中指定index的p
    this.person.splice(index,1)  //删除1个
}
```

M：那我想更改指定列的内容，怎么实现呢？

M：我现在存在一个数组，直接用``arr[0]='new content'``虽然替换成功，当显示的内容并不会响应式更新，怎么解决呢？

Z：vue提供了观察数组的方法，使用以下代码即可进行响应式替换：

```html
<button @click="updateP(index,{name:'Cat',age:20})">
    更新
</button>
```

```javascript
updateP(index,newP){
    this.persons.splice(index,1，newP)  
}
```

M：那我要添加一条数据怎么实现呢？

Z：改数字1为0就可以了``this.persons.splice(index,0，newP)  ``

M：如果实时更新的是对象而不是数组，该怎么实现呢？

Z：使用代码``this.$set(this.infoData, 'attachment', dataTemp.list)``即可

#### 对象

M：如果要更新属性的是一个对象，用以下代码虽然成功更新，也不能响应式更新，该怎么做：

```javascript
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

Z：换一种方式：

```javascript
var vm = new Vue({
  el: '#v-for-object',
  data: {
	man:{
		name:'liuXin',
		age : 13
	}
  },
  methods:{
	test:function(){
		vm.man = Object.assign({}, vm.man, {
		  height: 180,
		  favoriteColor: 'Vue Green'
		})
		console.log(this.man);
	}
  }
})
```

### 6.自定义指令

自定义指令可以使用全局指令，也可以使用局部指令：

全局指令：

```vue
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

局部指令：

```vue
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

该指令提供的钩子函数如下：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

- `componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- `unbind`：只调用一次，指令与元素解绑时调用。

## vue自带实例

Z：在vue中，系统暴露了一些自带的属性了实例，用``$``前缀与用户自定义属性进行区分。

#### $watch

M：怎么一直监听某个变量的变化？

Z：代码如下：

```javascript
var obj = {mess:"即将被改变的内容"};
```

```javascript
// $watch 是一个实例方法
vm.$watch('mess', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
  alert("发现啦，你改变了");
})
```

#### filter方法

M：当有一些数据不想显示出来，一般这种过滤操作都是在后端处理完传到前台的。vue要用什么方式过滤掉呢？

Z：如下代码：

```html
<p>{{evenNumbers}}</p>
```

```javascript
  data: {
	numbers: [ 1, 2, 3, 4, 5 ]
  },
  computed: {
	  evenNumbers: function () {
		return this.numbers.filter(function (number) {
		  return number % 2 === 0
		})
	  }
  },
```

实现filter方法，判读boolean结果来决定是否将数据return到前端。

M：那我现在要实现实时过滤，应该怎么做呢？

Z：如下代码

```html
<div id="demo">
  <input type="text" v-model="searchName">
  <ul>
    <li v-for="(p,index) in filterPersons" :key="index">
      {{index}}--{{p.name}}--{{p.age}}
    </li>
  </ul>
</div>
```

```javascript
  new Vue({
    el: '#demo',
    data: {
      searchName: '',
      persons: [
        {name: 'Tom', age:18},
        {name: 'Jack', age:17},
        {name: 'Bob', age:19},
        {name: 'Mary', age:16}
      ]
    },

    computed: {
      filterPersons () {
        // 取出相关数据
        const {searchName, persons} = this
		//最终要显示的数组
		let fPersons;
        // 过滤数组        
		fPersons = persons.filter(p => p.name.indexOf(searchName)!==-1)  
        return fPersons
      }
    }
  })
```

使用``const {searchName, persons} = this``后，就不用写``this.searchName``了，直接调用即可。

filter将要显示的数据过滤后返回``fPersons = persons.filter(p => p.name.indexOf(searchName)!==-1)``p

默认为persons的一个元素person

M：如果我要实现排序，怎么做呢？

Z：用这个方法定义排序规则就可以了：

```javascript
fPersons.sort(function (p1, p2) {
    if(orderType===1) { // 降序
        return p2.age-p1.age
    } else { // 升序
        return p1.age-p2.age
    }
})
```

M：我要格式化日期，如果每一个都用怎么使用Filter格式化日期呢？

Z：代码如下：

```html
<div id="test">
  <p>最完整的: {{time | dateString}}</p>
  <p>年月日: {{time | dateString('YYYY-MM-DD')}}</p>
</div>
```

```javascript
  // 定义过滤器
  Vue.filter('dateString', function (value, format='YYYY-MM-DD HH:mm:ss') { //形参默认值
    return moment(value).format(format);
  })
```

把值和格式传到过滤器中，使用moment插件进行转换。

#### 原生DOM

M：如果要使用原生DOM，怎么做？

Z：代码如下：

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
</button>
```

```javascript
methods: {
    warn: function (message, event) {
        // 现在我们可以访问原生事件对象
        if (event) event.preventDefault()
        alert(message)
    }
}
```

使用$event可以将event作为参数传到方法中，通过解析event可以对DOM元素进行操作(例如获取value内容)。

#### 事件修饰符

Z：方法中只处理数据，而DOM事件vue自带了事件修饰符；用到时进行查询：

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>   //stop停止默认行为，prevent阻止事件冒泡

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

#### ES

M：let，const，var三者之间有什么区别呢？

Z：功能上的不同，var是全局的，let是当前代码块有效，而const定义的是常量。  

#### 箭头函数=>

M：箭头函数=>该怎么理解呢？

Z：如``var func = param => param.split(" ");``转化为一般函数就是：

```javascript
var func = function (param) {
    return param.split(" ");
}
```

它把return和方法名给省略掉了。

#### 三元运算符

M：如果想在标签上的属性做判断，该怎么实现呢？

Z：可以使用三元运算符

```javascript
<FormItem :label="subject.titbf.isqa === true ? '必填' : ''" :prop="subject.titbf.isqa === true ? 'radioProp' : ''" >
```

## [组件](components/vue-component.md)   

通过组件的方式可以将代码提取出来，进行复用。



## 规范

Z：对于代码的规范，可以使用命令``npm run lint``进行检测

### 1.css作用域

Z：为了不让自己写的代码不被第三方或者其他人的HTML/CSS影响，有以下两种方式可以设置样式作用域：

- 使用class策略：添加个前缀``c-``区分，例如以下代码：

  ```vue
  <template>
    <button class="c-Button c-Button--close">X</button>
  </template>
  
  <!-- 使用 BEM 约定 -->
  <style>
  .c-Button {
    border: none;
    border-radius: 2px;
  }
  
  .c-Button--close {
    background-color: red;
  }
  </style>
  ```

- 使用`scoped` 特性

  如``<style scoped>``或``<style module>``

### 2.私有属性名

Z：直接定义属性可能与其他人属性冲突，可以使用``$_spaceName_propertyName``以区分：

```javascript
var myGreatMixin = {
  // ...
  methods: {
    $_myGreatMixin_update: function () {
      // ...
    }
  }
}
```

### 3.提取公用方法  

M：怎么将共用方法提取出来呢？

Z：新建共用的js，代码如下：

```javascript
export default {
    /**
     * 手机号码规则
     * @param rule
     * @param value
     * @param callback
     * @returns {*}
     */
    validateTelPhone: function(rule, value, callback){
        if (value.length > 0 && value.length != 11) {
            return callback(new Error("不是有效的手机号码！"));
        }
        callback();
    }
}
```

Z：然后引入该js文件，声明变量，即可调用``common.validateTelPhone``

```javascript
import common from "../../../../utils/common.js";
import Vue from "vue";
Vue.prototype.common = common;
```

## 路由

### 1.新窗口跳转   

M：怎么新弹窗进行跳转呢？

Z：使用命令

```javascript
openExamineListWin() {
    window.open(window.location.origin + "/infoMaintain/ExamineList"); //打开调查问卷列表
}
```

### 2.路由跳转   

M：怎么设置路由呢？

Z：分以下三个步骤

1. 设置路由

   ```javascript
   Vue.use(Router)
   
   export default new Router({
     routes: [
       {
         path: '/goods',
         component: goods
       },
       {
         path: '/ratings',
         component: ratings
       },
   ...
     ]
   })
   ```

2. 加载路由

   ```javascript
   import router from './router'
   
   new Vue({ //配置对象的属性都是固定的属性名
     el: '#app',
     components: {App},
     template: '<App/>',
     router
   })
   ```

3. 使用路由

   ```javascript
   <div class="tab-item">
       <router-link to="/ratings">评论</router-link>
   </div>
   <div class="tab-item">
       <router-link to="/seller">商家</router-link>
   </div>
   <router-view></router-view>
   ```

M：怎么在路由跳转的时候，传递参数呢？

Z：步骤如下

1. 在路由配置中配置带参数

   ```javascript
   path: "onlineDistrictAcceleratorForm/:id",
   ```

2. 跳转时传递参数进去

   ```javascript
   viewInfo() {
       this.$emit("listen");
       this.$router.push({
           path: this.viewPath + "/" + this.busId
       });
   }
   ```

3. 在跳转的页面获取参数

   ```javascript
   mounted() {
       //判断编辑或新增
       let id = this.$route.params.id;
   }
   ```

## 数据提交   

Z：调用Api时数据提交的代码如下：

```javascript
if (that.code != "" && that.code != undefined) {
    data = (await Api.getExamineByCode({
        code: that.code
    })).data;
} else {
    data = (await Api.getExamine({})).data;
}
```

Z：提交对象，直接提交该属性即可

```javascript
                        Api.saveExamineAnswer({
                            res: sub.code + "," + sub.titNo
                        });
```

M：如果要提交的是表单数据和字符串混合，怎么实现呢？

Z：如下代码：

```javascript
            let data = (await Api.getIncubatorEntpMonthList({
                ...that.queryParams,
                current: that.currentPage,
                size: that.showSize,
                sort: "desc"
            })).data;
```

...可以直接序列化表单数据，配合字段可以实现多参数提交

## 初始化数据

M：怎么回显数据呢？

Z：调用初始化页面方法，然后将获取到数据存进定义的data之中

```javascript
    mounted() {
        //初始化公告数据
        if (this.code !== "") {
            //不判断直接执行会报错
            this.initBbs();
        }
    },
```

```javascript
    methods: {
        async initBbs() {
            let that = this;
            let data;
            if (that.code != "" && that.code != undefined) {
                data = (await Api.getBbsByCode({
                    code: that.code
                })).data;
            }
            that.bbs = data;
        }
    },
```

回显数据需要添加async异步，await同步请求。

## 异常处理   

M：当服务器端抛出异常，前端怎么获取到呢？

Z：如下代码

```javascript
let status = (await Api.saveSuggest({
    formData: formData,
    machineCode: that.guid,
    validateCode: that.formItem.validateCode})
   .catch(error => {
        let loginMsg =
            error.message === "" ? "服务异常" : error.message;
        that.$Message.warning(loginMsg);
    })
    .finally(() => {
        that.refreshValidateCode();
    })
).status;
```

使用.catch捕捉error变量，从中获取参数进行判断。

## 数据绑定

vue是单向绑定的，通过语法糖实现双向绑定

```vue
<PersonalInfo v-model="phoneInfo" :zip-code.sync="zipCode" />
```

编译后

```vue
<PersonalInfo
	:phone-info="phoneInfo"
    @change="val => (phoneInfo = val)"
    :zip-code="zipCode"
    @update:zipCoe="val => (zipCode = val)"/>
```

## 生命周期

创建阶段的步骤：

1. beforeCreate方法
2. data:function() {}   
3. created
4. beforeMount
5. render  
6. mounted

更新的步骤（会多次执行）：

1. beforeUpdate
2. render
3. updated

销毁阶段的步骤：

1. beforeDestroy
2. destroyed

## 跨层节点操作

获取节点元素方式如下：

```html
<p ref="p">hello</p>	<!-- vm.$refs.p -->
<child-component ref="child"></child-component>   <!-- vm.$refs.child -->
```

当节点层数比较多，怎么操作隔好几层节点的元素，分为两种情况：

- 获取：无需v-ant-ref属性

  父节点提供方法：

  ```vue
    provide() {
      return {
        getChildrenRef: name => {
          return this[name];
        },
  ...
  ```

  子节点注入方法：

  ```vue
    inject: {
      getParentRef: {
        from: "getRef",
        default: () => {}
      },
  ...
  ```

  可以直接获取父类DOM元素

- 设置：需要v-ant-ref通知

  设置除了需要提供、注入。子节点还需要设置v-ant-ref，在数据更新的时候告知父节点，父节点进行缓存。









​          



