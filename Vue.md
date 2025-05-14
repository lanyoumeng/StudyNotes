# 前端



## HTML



## css



## js



------



# Vue应用

```Plaintext
//vue应用
const App = Vue.createApp({})
```

Vue 应用通常由以下几个关键部分定义：

### 1. 根实例（Root Instance）：

在一个 Vue 应用中，通常会创建一个根 Vue 实例。这个实例负责管理整个应用，并挂载到一个 HTML 元素上。

```JavaScript
const app = new Vue({
  // 配置选项
});
app.$mount('#app'); // 将根实例挂载到页面的某个元素上
```

### 2. 选项（Options）：

根实例可以包含一系列的配置选项，用来定义应用的行为、数据、模板等。

常见的选项包括：

- **data：** 用于定义应用的数据。
- **methods：** 包含了应用中的方法和函数。
- **computed：** 计算属性，根据其他数据的变化计算出新的值。
- **watch：** 监听数据的变化执行相应的操作。
- **template：** 定义视图模板。

### 3. 组件（Components）：

Vue 应用可以由多个组件构成，每个组件是一个独立的 Vue 实例，负责管理自己的视图、数据和行为。组件可以被嵌套使用，形成组件树。

```JavaScript
Vue.component('my-component', {
  // 组件选项
});
```

### 4. 路由（Router）：

对于单页应用（SPA），Vue.js 通常会使用 Vue Router 管理页面的路由和导航。Vue Router 允许定义不同 URL 对应的组件，实现页面间的切换和导航。

```JavaScript
const router = new VueRouter({
  routes: [
    { path: '/', component: Home },
    { path: '/about', component: About }
    // ...其他路由
  ]
});
```

### 5. 状态管理（State Management）：

在大型应用中，可能会使用 Vuex 这样的状态管理库来集中管理应用的状态（数据）。Vuex 提供了一种集中式存储管理应用所有组件的状态，并提供了规则保证状态只能按照一定的方式进行修改。

### 6. 生命周期钩子（Lifecycle Hooks）：

Vue 提供了一系列的生命周期钩子函数，允许开发者在实例生命周期的不同阶段执行自定义逻辑。常见的生命周期钩子包括 `created`、`mounted`、`updated` 和 `destroyed` 等。

通过这些部分的定义和组织，Vue 应用可以变得结构清晰、可维护，并且能够更好地处理数据和视图之间的关系，实现动态且响应式的用户界面。

### 7.应用绑定

![image-20231215162824490](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112010069.png)

# 特点和介绍：

1. **轻量级和易学**：Vue.js是一个轻量级框架，容易学习和上手，即使您没有深厚的前端经验也能使用。
2. **响应式数据绑定**：Vue.js使用双向数据绑定，允许您将数据绑定到视图，当数据变化时，视图会自动更新，反之亦然。
3. **组件化**：Vue.js支持组件化开发，允许您将用户界面拆分为多个可重用的组件，每个组件都有自己的状态和行为。
4. **虚拟DOM**：Vue.js使用虚拟DOM来高效地更新视图，减少对DOM的直接操作，提高性能。
5. **指令和模板**：Vue.js提供了一系列的指令（如`v-bind`、`v-model`、`v-for`等）和模板语法，使模板编写和数据绑定变得更加简单。
6. **生命周期钩子**：Vue组件具有生命周期钩子，允许您在不同阶段执行自定义逻辑，例如`created`、`mounted`、`updated`等。
7. **路由管理**：Vue Router是Vue.js的官方路由库，用于构建SPA的客户端路由。
8. **状态管理**：Vue.js提供了Vuex，用于管理应用的状态（状态管理模式），以便更好地处理大型应用程序中的状态和数据流。

以下是一个简单的Vue.js示例，展示了如何创建一个基本的Vue实例以及如何在模板中绑定数据：

```HTML
<!DOCTYPE html>
<html>
<head>
  <title>Vue.js Example</title>
</head>
<body>
  <div id="app">
    <h1>{{ message }}</h1>
    <input v-model="message" placeholder="Type something...">
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        message: 'Hello, Vue!'
      }
    });
  </script>
</body>
</html>
```

在上述示例中，我们创建了一个Vue实例，并将其绑定到具有`id="app"`的DOM元素上。然后，我们定义了一个数据属性`message`，并在模板中使用`{{ message }}`将其显示在页面上。另外，我们使用`v-model`指令使输入框与`message`进行双向数据绑定。

这是一个非常简单的Vue.js示例，但Vue.js支持更复杂的应用程序构建，包括路由、状态管理、组件化等。您可以根据需要逐渐扩展您的Vue应用程序。

# 虚拟DOM（Virtual DOM）

是一种用于优化Web应用性能的前端开发技术。虚拟DOM是在内存中构建的虚拟树结构，用于表示页面的结构和内容。它是许多现代前端框架和库（如React、Vue.js）的核心概念。

以下是关于虚拟DOM的主要概念和它如何工作的简要解释：

1. **虚拟DOM是什么**：虚拟DOM是一个JavaScript对象树，它在内存中表示了实际DOM的结构和内容。它是一个轻量级的数据结构，由节点和属性组成，不包含实际的渲染逻辑。
2. **为什么使用虚拟DOM**：虚拟DOM的主要目标是提高页面渲染性能。实际DOM的更新是昂贵的操作，而虚拟DOM通过将多次变更合并成单个变更，然后批量处理，减少了DOM操作的次数，从而提高了性能。
3. **虚拟DOM工作原理**：当数据发生变化时，应用程序会创建一个新的虚拟DOM树，表示新的状态。然后，新的虚拟DOM树与先前的虚拟DOM树进行比较，找出差异。这个过程称为"协调"（Reconciliation）或"调和"（Reconciliation）。然后，只有差异部分会被应用到实际DOM中，从而更新视图。
4. **性能优势**：通过减少实际DOM操作的次数，虚拟DOM可以提高页面渲染性能。它还使得跨平台渲染（如React Native）更容易实现，因为虚拟DOM可以适应不同的渲染目标。
5. **框架和库的支持**：虚拟DOM已成为许多流行前端框架和库的核心，例如React使用虚拟DOM来进行高效的渲染，Vue.js也使用虚拟DOM来实现其渲染优化。
6. **开发者无感知**：虚拟DOM通常是框架内部实现的，因此开发者在编写应用程序时无需手动操作虚拟DOM。它是一种潜在的性能优化，可以透明地提高应用程序性能。

总之，虚拟DOM是一种用于提高Web应用性能的前端开发技术，通过在内存中构建虚拟DOM树，然后进行高效的差异比较和更新，以减少实际DOM操作的次数，从而提高页面渲染性能。它是现代前端框架和库的核心概念之一，有助于开发者构建高性能、交互式的Web应用。

# 全局API

![image-20240113192256544](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112011034.png)

![image-20240113192742948](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112011721.png)

![image-20240113192754575](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112011271.png)

# 模板

是用于构建用户界面的核心概念。

模板则定义了应用的布局和结构，指令是用于在模板中声明性地将数据绑定到DOM元素或应用行为的特殊标记。

## 响应式数据绑定

是Vue.js的一个核心概念，它允许您在模板中绑定数据，并确保当数据发生变化时，视图会自动更新以反映这些变化。这是通过Vue.js的双向数据绑定机制实现的

1. **数据属性**：您可以在Vue组件的`data`选项中定义数据属性，这些属性将在模板中使用。

```JavaScript
data() {
  return {
    message: "Hello, Vue!"
  };
}
```

1. **模板绑定**：在模板中，您可以使用双括号语法(`{{ message }}`)将数据属性绑定到视图，这样数据将显示在页面上。

```HTML
<div id="app">
  <p>{{ message }}</p>
</div>
```

1. **双向数据绑定**：Vue.js还提供了`v-model`指令，用于实现双向数据绑定，可以用于表单元素。

```HTML
<div id="app">
  <input v-model="message" placeholder="Type something...">
  <p>{{ message }}</p>
</div>
```

1. **方法和计算属性**：您可以在Vue组件中定义方法和计算属性来处理数据的变化和计算。

```JavaScript
methods: {
  changeMessage() {
    this.message = "New message";
  }
},
computed: {
  reversedMessage() {
    return this.message.split('').reverse().join('');
  }
}
```

1. **自动更新**：当数据属性发生变化时，Vue.js会自动更新视图，这意味着无需手动操作DOM来反映数据的变化。

```JavaScript
this.message = "New message"; // 视图会自动更新
```

1. **数据响应性**：Vue.js使用虚拟DOM和数据观察机制来实现数据的响应性，以最小化DOM更新操作，提高性能。

响应式数据绑定的核心思想是将数据和视图保持同步，当数据发生变化时，视图会相应地更新，而当视图发生变化时，数据也会同步更新。这使得构建动态、交互式的用户界面变得非常方便，并帮助减少了手动DOM操作的工作。Vue.js的这种机制使得前端开发更加高效和容易维护。

## 模板插值

`{}` 用于静态属性绑定，`{{}}` 用于文本内容插值，`v-bind` 用于动态属性绑定，而 `v-model` 用于建立双向数据绑定。

1. `{}`：花括号 `{}` 通常用于模板中的属性绑定，特别是用于静态属性的绑定。在Vue模板中，您可以使用对象字面量 `{}` 来指定元素的属性，其中对象的键表示属性名，对象的值表示属性值。

```Plaintext
<template>
  <div :class="{ active: isActive, 'text-danger': hasError }">
    <!-- ... -->
  </div>
</template>
```

在上述示例中，`:class` 指令使用对象字面量来动态绑定元素的类属性。

1. `v-bind`：`v-bind` 是Vue.js提供的指令，用于动态地绑定属性的值。您可以使用 `v-bind` 来将属性绑定到组件的数据或表达式。语法是 `v-bind:属性名="表达式"`。

```Plaintext
<template>
  <a v-bind:href="url">Visit Google</a>
</template>

快捷方式：  
<template>
  <a :href="url">Visit Google</a>
</template>
```

在上面的示例中，`v-bind:href` 指令将 `url` 数据属性的值绑定到链接的 `href` 属性。

```Plaintext
<h1 v-bind:[prop]="name" v-if="show">标题</h1>
show : true,
prop : "class" ,
name : "hl"
布尔变量控制其设置的样式是否被选用
.red {color : red }
.blue {color: blue }

<div :class=" { blue :isBlue , red:isRed } ">

data ( ) {
return {
isBlue :true,isRed: false,
}}


==    
HTML元素:
<div :class="style">
示例文案
< / div>Vue组件:
const App = {
data () {
return {
style : {
blue : true,red: false
}
}
}
}



Vue还支持使用数组对象来控制class属性，示例如下:HTML元素:
<div :class=" [redClass, fontClass ] ">
示例文案
</ div>
```

1. `{{}}`：花括号 `{{}}` 用于插值，它允许您在模板中嵌入变量或表达式的值，从而将其渲染到页面中。这主要用于在模板中显示文本内容，而不是绑定属性。
2. 双括号会将其内的变量解析成纯文本
3. `v-model`：`v-model` 是Vue.js提供的双向数据绑定指令，用于在表单元素（如输入框、单选按钮、复选框等）和组件的数据属性之间建立双向绑定。

```Plaintext
<template>
  <input v-model="message">
</template>

//修饰符 当用户在输入框中输入的文本首尾有空格符时，以及输入框失去焦点时，Vue会自动帮我们去掉这些首尾空格。
<input v-model.trim="content">

lazy修饰符的作用有些类似于属性的懒加载。
  <input v-model.lazy="textField"/>
     <p>文本输入框内容:{{textField}}</p>
运行上面的代码，只有当用户完成输入，即输入框失去焦点后，段落中才会同步到输入框中最终的文本数据
```

1. v-once指令实现，被这个指令设置的组件在进行变量插值时只会插值一次，例如：

```Plaintext
     <h1 v-once>这里是模板的内容:{{count}}次点击</h1>
```

6.V-html指令可以指定一个Vue变量数据，其会通过HTML解析的方式将原始HTML替换到其指定的标签位置

```Plaintext
  <h1 v-once>这里是模板的内容:<span v-html="countHTML"></span>次点击</h1>
```

## 模版指令

![image-20240113173500005](D:\code\typora-user-images\image-20240113173500005.png)

![image-20240113173509230](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112012419.png)

指令是以`v-`开头的特殊属性，用于向DOM元素添加声明性的行为。指令将Vue实例中的数据绑定到DOM元素，使数据和视图之间保持同步。以下是一些常见的Vue指令：

1. **`v-for`**：用于渲染循环中的元素，允许您遍历数组或对象并生成多个元素。

```css
<ul>
  <li v-for="item in items">{{ item }}</li>
</ul>
```

![image-20231215093034787](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112012810.png)

![image-20231215093207473](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112013694.png)

1. 在实际开发中，原始的列表数据往往并不适合直接渲染到页面，v-for指令支持在渲染前对数据进行额外的处理，修改标签如下：

   ![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112027315.jpeg)

   上面的代码中，handle为定义的处理函数，在进行渲染前，通过这个函数来对列表数据进行处理。例如，我们可以使用过滤器来进行列表数据的过滤渲染，实现handle函数如下：

   ![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112027857.jpeg)

   当需要同时循环渲染多个元素时，与v-if指令类似，常用的方式是使用template标签进行包装，例如：

   ![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024698.jpeg)

2. **`v-if、v-else-if、v-else`**：用于条件性地渲染元素，根据表达式的值来决定元素是否显示。

   设置了v-else指令的元素必须紧跟在v-if或v-else-if指令指定的元素后面，否则其不会被识别到。

   ```vue
     <h1 v-if="mark == 100">满分</h1>
        <h1 v-else-if="mark > 60">及格</h1>
        <h1 v-else>不及格</h1>
   ```

3. v-if指令的使用必须添加到一个HTML元素上，如果我们需要使用条件同时控制多个标签元素的渲染

   v-if采取的是懒加载的方式进行渲染，如果初始条件为假，则关于这个组件的任何渲染工作都不会进行，直到其绑定的条件为真时，才会真正开始渲染此元素。

   使用template进行分组的组件渲染后并不会渲染template标签本身

   ```vue
   <template v-if="show">
   <p>内容</p>
   <p>内容</p><p>内容</p>
   </ template>
   ```

4. v-show:

   与v-if不同的是，v-show并不支持template模板，同样也不可以和v-else结合使用。

   无论v-show指令设置的条件是真是假，当前元素都会被渲染，v-show指令只是简单地通过切换元素CSS样式中的display属性来实现展示效果。

   ```vue
        <h1 v-show="show">v-show标题在这里</h1>
   ```

   如果组件的渲染条件会比较频繁地切换，则建议使用v-show指令来控制，如果组件的渲染条件在初始指定后就很少变化，则建议使用

   v-if指令控制。



## 事件修饰符

![image-20240113173944930](C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20240113173944930.png)





## 自定义指令

### 全局注 册

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024721.jpeg)

如以上代码所示，调用应用示例的directive方法可以注册全局的自定义指令。上面的代码中，getfocus是指令的名称，在使用时需要加上“v-”前缀。运行上面的代码，可以看到页面被加载时其中的输入框默认处于焦点状态，可以直接进行输入。

在自定义指令时，通常需要在组件的某些生命周期节点进行操作，自定义指令中除了支持mounted生命周期方法外，也支持使用beforeMount、beforeUpdate、updated、beforeUnmount和unmounted生命周期方法，我们可以选择合适的时机来实现自定义指令的逻辑。

上面的示例代码中采用全局注册的方式来自定义指令，因此所有组件都可以使用，



### 局部注册

在组件内部进行directives配置来定义自定义指令，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112027076.jpeg)



### 自定义指令的参数

自定义指令也可以设置值和参数，这些设置数据会通过一个param对象传递到指令中实现的生命周期方法中，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024713.jpeg)

值1被绑定到param对象的value属性
custom参数被绑定到param对象的arg属性

有了参数，Vue自定义指令的使用可以非常灵活，通过不同的参数进行区分，我们可以很方便地处理复杂的组件渲染逻辑。

对于指令设置的值，其也允许直接设置为JavaScript对象，例如下面的设置也是合法的：

```
     <input v-getfocus:custom="{a:1, b:2}" />
```

## 组合式API

传统的Vue组件的开发方式需要将数据、方法、侦听等逻辑分别配置在不同的选项中，这使得对于完成一个独立的功能，其逻辑关注点要分别写在不同的地方，不利于代码的可读性。Vue 3中引入了组合式API的开发方式，开发者在编写组件时可以在setup方法中将组件的逻辑聚合地编写在一起。

#### 关于setup方法

setup方法是Vue 3中新增的方法，属于Vue 3的新特性，同时它也是组合式API的核心方法。

setup方法是组合式API功能的入口方法，如果使用组合式API模式进行组件开发，则逻辑代码都要编写在setup方法中。需要注意，setup方法会在组件创建之前被执行，即对应组件的生命周期方法beforeCreate方法调用之前被执行。由于setup方法特殊的执行时机，除了可以访问组件的传参外部属性props之外，在其内部我们不能使用this来引用组件的其他属性，在setup方法的最后，我们可以将定义的组件所需要的数据、函数等内容暴露给组件的其他选项（比如生命周期函数、业务方法、计算属性等）。接下来，先深入了解一下setup方法。

首先创建一个名为setup.html的测试文件用来编写本节的示例代码。setup方法可以接收两个参数：props和context。props是组件使用时设置的外部参数，其是响应式的；context则是一个JavaScript对象，其中可用的属性有attrs、slots和emit。示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112027132.jpeg)

在setup方法的最后可以返回一个JavaScript对象，此对象包装的数据可以在组件的其他选项中使用，也可以直接用于HTML模板中，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028178.jpeg)

如果不在组件中定义template模板，也可以直接使用setup方法来返回一个渲染函数，当组件将要被展示时，会使用此渲染函数进行渲染。将上面的代码改写如下会有一样的运行效果：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024265.jpeg)

最后，再次提醒，在setup方法中不要使用this关键字，setup方法中的this与当前组件实例并不是同一对象。

#### setup方法中定义生命周期

setup方法本身也可以定义组件的生命周期方法，方便将相关的逻辑组合在一起。setup方法常用的生命周期定义方法如表7-1所示（在组件的原生命周期方法前加on即可）。

表7-1　setup方法常用的生命周期定义方法

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028957.jpeg)

你可能发现了，在表7-1中，我们去掉了beforeCreate和created两个生命周期方法，这是因为从逻辑上来说，setup方法的执行时机与这两个生命周期方法的执行时机基本是一致的，在setup方法中直接编写逻辑代码即可。

下面的代码将演示在setup方法中定义组件生命周期的方法：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028933.jpeg)

需要注意，如果组件和setup方法中都定义了同样的生命周期方法，它们之间并不会冲突。在实际调用时，会先调用setup方法中定义的，再调用组件内部定义的。



## 响应式对象

对于对象类的数据，可以使用reactive进行包装，对于直接的简单数据，可以使用ref方法进行包装。需要注意，使用ref包装的数据在setup方法中访问时，需要使用其内部的value属性进行访问，当我们将ref数据返回在模板中进行使用时，则会默认进行转换，可以直接使用。除了使用reactive和ref方法来包装数据外，也可以使用computed方法来定义计算数据，以实现响应式。

### setup

setup方法可以在组件被创建前定义组件需要的数据和函数



### 创建

#### reactive

vue 3.0中提供了reactive方法，使用这个方法对自定义的JavaScript对象进行包装，即可以方便地为其添加响应式。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024811.jpeg)

再次运行代码，当myData中的value属性发生变化时，可以同步进行页面元素的刷新了。

#### ref方法

一个独立的原始值，可以直接使用Vue提供的ref方法来定义响应式独立值，ref方法会帮我们完成对象的包装
myObject有value属性值，接收一个参数作为初始值。
模板中使用setup返回的使用ref定义的数据时，数据对象会被自动展开。

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024521.jpeg" alt="img" style="zoom:50%;" />





//////

默认不支持响应式变量的解构赋值
改写后的代码可以正常运行，并且能够正确获取到value变量的值，但是需要注意，value变量已经失去了响应式，对其进行的修改无法同步刷新页面。


![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028899.jpeg)



///////

#### toRefs

支持响应式对象的解构赋值
可以直接将JavaScript对象中的属性进行解构，从而直接赋值给变量使用。
有一点需要注意，Vue会自动将解构的数据转换成ref对象变量，因此在setup方法中使用时，要使用其内部包装的value属性。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028765.jpeg)





### 计算与监听

#### 计算

1.在组件中可以使用computed选项来定义计算属性。

2.Vue中也提供了一个同名的方法，可以直接使用其创建计算变量。

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028365.jpeg" alt="img" style="zoom: 50%;" />







#### 监听

##### watchEffect

方法可以自动对其内部用到的响应式变量进行变化监听，由于其原理是在组件初始化时对所有依赖进行收集，因此在使用时无须手动指定要监听的变量，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024073.jpeg)

在调用watchEffect方法时，其会立即执行传入的函数参数，并会追踪其内部的响应式变量，在其变更时再次调用此函数参数。

需要注意，watchEffect在setup方法中被调用后，其会和当前组件的生命周期绑定在一起，组件卸载时会自动停止监听，如果需要手动停止监听，方法如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024499.jpeg)





##### watch

是一个与watchEffect类似的方法，与watchEffect方法相比，watch方法能够更加精准地监听指定的响应式数据的变化，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024630.jpeg)

watch方法比watchEffect方法强大的地方在于，其可以分别获取到变化前的值和变化后的值，十分方便地做某些与值的比较相关的业务逻辑。从写法上来说，watch方法也支持同时监听多个数据源，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028949.jpeg)







------



## 事件

处理用户交互实际上就是对用户操作事件的监听和处理。在Vue中，使用v-on指令来进行事件的监听和处理，更多时候我们会使用其缩写方式@代替v-on指令。
对于网页应用来说，事件的监听主要分为两类：键盘按键事件和鼠标操作事件。

**`v-on`**：用于监听DOM事件，当事件触发时，执行Vue实例中的方法。

```vue
<button v-on:click="handleClick">Click me</button>.

快捷方式：
<button @click="handleClick">Click me</button>.
```

#### 1

<img src="C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215155140535.png" alt="image-20231215155140535" style="zoom:67%;" />

<img src="C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215155151507.png" alt="image-20231215155151507" style="zoom:67%;" />

多事件：

<img src="C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215155252027.png" alt="image-20231215155252027" style="zoom: 67%;" />



#### 2

事件捕获：触发了一个单击事件时，从父组件开始依次传递到子组件
事件冒泡（默认v-on）：当事件传递到最上层的子组件时，其还会逆向地再进行一轮传递，从子组件依次向下传递。

![image-20231215160348967](C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215160348967.png)

![image-20231215160502418](C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215160502418.png)



![image-20231215160608321](C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215160608321.png)

对于每一种类型的事件，我们都可以通过Event对象来获取事件的具体信息，例如在鼠标单击事件中，可以获取到用户具体单击的是左键还是右键。

#### 键盘按键修饰符：

当需要对键盘按键进行监听时，我们通常使用keyup参数，如果仅仅要对某个按键进行监听，可以通过Event对象来判断，例如要监听用户敲击了回车键，方法可以这么写：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024462.jpeg)

在Vue中，还有一种更加简单的方式可以实现对某个具体的按键的监听，即使用按键修饰符，在绑定监听方法时，我们可以设置要监听的具体按键，例如：

```vue
     <input @keyup.enter="keyup"></input>
```

需要注意，修饰符的命名规则与Event对象中属性key值的命名规则略有不同，Event对象中的属性采用的是大写字母驼峰法，如Enter、PageDown，在使用按键修饰符时需要将其转换为中画线驼峰法，如enter、page-down。



特殊的系统按键修饰符，只有当用户按下这些键时，对应的键盘或鼠标事件才能触发，在处理组合键指令时经常会用到，主要有如下4种：

ctrl、alt、shift、meta

     <div @mousedown.ctrl="mousedown">鼠标按下</div>

用户按下Ctrl键的同时，再按下鼠标按键才会触发绑定的事件函数。

还有一个细节需要注意，上面示例的系统修饰符只要满足条件就会触发，以鼠标按下事件为例，只要满足用户按下了Ctrl键的时候按下了鼠标按键，就会触发事件，即使用户同时按下了其他按键也不会受影响，例如用户使用Shift+Ctrl+鼠标左键的组合按键。如果想要精准地进行按键修饰，可以使用exact修饰符，使用这个修饰符修饰后，只有精准地满足按键的条件才会触发事件，例如：

```
     <div @mousedown.ctrl.exact="mousedown">鼠标按下</div>
```



#### 鼠标按键修饰符

在进行网页应用的开发时，通常左键用来选择，右键用来进行配置。通过下面这些修饰符可以设置当用户按了鼠标指定的按键后才会触发事件函数：

left、right、middle

例如下面的示例代码，只有按了鼠标左键才会触发事件：

```
     <div @click.left="click">单击事件</div>
```



鼠标Event事件坐标属性及其含义

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028420.jpeg)







# 组件化



Vue框架将常规的网页页面开发以面向对象的方式进行了抽象，一个网页甚至一个网站在Vue中被抽象为一个应用程序。一个应用程序中可以定义和使用很多个组件，但是需要配置一个根组件，当应用程序被挂载渲染到页面时，此根组件会作为起点元素进行渲染。

是现代前端开发中的一个重要概念，它指的是将应用程序拆分成独立、可重用的组件，每个组件都有自己的状态、行为和视图。Vue.js在组件化方面表现出色，以下是有关Vue.js中组件化的关键概念和示例：



## **组件定义**

**组件**是可复用的、独立的Vue实例，包含了**模板（template）、逻辑（script）和样式（style）**，用来构建可组合的、模块化的页面。

组件在定义时的配置选项与Vue应用实例在创建时的配置选项是一致的，都有data、methods、watch和computed等配置项。这是因为我们在创建应用时传入的参数实际上就是根组件。
当组件进行复用时，每个标签实际上都是一个独立的组件实例，其内部的数据是独立维护的，

### 1.字符串模板、全局注册

当我们创建好了Vue应用实例后，使用mount方法可以将其绑定到指定的HTML元素上。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024053.jpeg)

如以上代码所示，在Vue应用中定义组件时使用component方法，这个方法的第1个参数用来设置组件名，第2个参数进行组件的配置，组件的配置选项与应用的配置选项基本一致。
定义组件是最重要的是template选项，这个选项设置组件的HTML模板
之后，当需要使用自定义的组件时，只需要使用组件名标签即可，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028524.jpeg)

需要注意，上面代码中的my-alert组件是定义在Application应用实例中的，在组织HTML框架结构时，my-alert组件也只能在Application挂载的标签内使用，在外部使用是无法正常工作的，



### 2.单文件组件、局部注册

组件的模板可以是字符串模板，也可以使用单文件组件（`.vue` 文件），单文件组件将模板、逻辑和样式分离开，更易于组织和维护。

```vue
<template>
  <div>
    <!-- 组件模板 -->
  </div>
</template>

<script>
export default {
  // 组件选项
};
</script>

<style>
/* 组件样式 */
</style>
```

在 Vue 应用中，你可以在需要使用组件的地方引入并注册单文件组件：

```vue
<template>
  <div>
    <my-component></my-component>
  </div>
</template>

<script>
import MyComponent from './MyComponent.vue'; // 引入单文件组件

export default {
  components: {
    MyComponent // 注册组件
  }
};
</script>
```







## **组件注册**

定义组件后，您需要在父组件中注册它们。注册的方式可以是全局注册或局部注册。

- 全局注册：

  直接使用应用实例的component方法注册的组件都是全局组件，即可以在应用内的任何地方使用这些组件，包括其他组件内部，

```javascript
Vue.component('my-component', MyComponent);
```

- 局部注册：

```vue
<script>
import MyComponent from './MyComponent.vue';

export default {
  components: {
    'my-component': MyComponent
  }
}
</script>

/////
const ParentComponent = {
  components: {
    'my-component': {
      // 组件选项
    }
  },
  // ...
};

```



## 组件配置

当调用Vue.createApp方法后，会创建一个Vue应用实例，对于此应用实例，其内部封装了一个config对象，我们可以通过这个对象的一些全局选项来对其进行配置。常用的配置项有异常与警告捕获配置和全局属性配置。

在Vue应用运行过程中，难免会有异常和警告产生，我们可以使用自定义的函数来对抛出的异常和警告进行处理。示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024274.jpeg)

之前，我们在使用组件时，组件内部使用到的数据要么是组件内部自己定义的，要么是通过外部属性从父组件传递进来的。在实际开发中，有些数据可能是全局的，例如应用名称、应用版本信息等，为了方便地在任意组件中使用这些全局数据，可以通过globalProperties全局属性对象进行配置，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030082.jpeg)

## **组件嵌套**

您可以在模板中嵌套组件，构建复杂的用户界面。子组件可以包含在父组件中，形成嵌套关系。

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <my-component></my-component>
  </div>
</template>
```



## 属性和方法

在Vue.js中，组件是可重用的，具有属性和方法，允许您创建独立的UI单元并将其组合在一起以构建应用程序。以下是有关Vue组件属性和方法的主要概念：

### data

 选项用于定义组件的数据属性。`data` 选项可以有两种不同的语法：

1. **函数形式的 `data`**，如您提供的示例：

```vue
data() {
  return {
    messageFromParent: "Hello from parent",
  };
}
```

2. **对象形式的 `data`**，如下所示：

```vue
data: {
  messageFromParent: "Hello from parent",
}
```

在实际应用中，这两种形式的区别在于函数形式的 `data` 具有一些特殊的行为，它会为每个组件实例返回一个独立的数据对象，从而避免了组件之间共享数据。而对象形式的 `data` 会在多个组件实例之间共享同一个数据对象，这可能会导致数据共享问题。

因此，通常建议在 Vue 组件中使用函数形式的 `data`，以确保数据的独立性。这是 Vue.js 组件的最佳实践。

### Methods

1. **Methods（方法）**：组件可以包含方法，这些方法可以在组件内部和从组件的模板中调用。方法通常用于处理用户交互、触发事件、计算属性等。

```vue
<template>
  <div>
    <button @click="incrementCounter">Increment Counter</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter() {
      this.counter++;
    }
  }
}
</script>
```

#### 使用计算属性还是函数

计算属性是基于其所依赖的存储属性的值的变化而重新计算的，计算完成后，其结果会被缓存，下次访问计算属性时，只要其所依赖的属性没有变化，其内的逻辑代码就不会重复执行。而函数则不同，每次访问其都会重新执行函数内的逻辑代码得到的结果。因此，在实际应用中，我们可以根据是否需要缓存这一标准来选择使用计算属性或函数。



### Computed 

**计算属性**：计算属性是一种根据组件的属性或状态计算派生值的方法。它们类似于方法，但可以像属性一样访问。

您可以通过计算属性来派生新的数据，而不是直接在模板中计算。

```vue
<template>
  <div>
    <p>{{ messageLength }}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: String
  },
  computed: {
    messageLength() {
      return this.message.length;
    }
  }
}
</script>

////////###
<template>
  <div>
    <p>{{ reversedMessage }}</p>
  </div>
</template>

<script>
export default {
  computed: {
    reversedMessage() {
      return this.message.split('').reverse().join('');
    }
  }
}
</script>



```



存数据的方法我们需要手动实现，通常称之为set方法。

![image-20231215104344943](C:\Users\to'm\AppData\Roaming\Typora\typora-user-images\image-20231215104344943.png)



### Watchers

4. **Watchers（监听器）**：Watcher 允许您监听组件属性或数据的变化，并在数据变化时执行自定义逻辑。它可以用于响应属性的变化，执行异步操作等。

```vue
<script>
export default {
  props: {
    message: String
  },
  watch: {
    message(newValue, oldValue) {
      // 处理属性 message 的变化
    }
  }
}
</script>
```



###  filters

在Vue.js中，过滤器是一种用于对数据进行处理并以特定格式显示的功能。过滤器可以在两个地方使用：在Mustache插值和`v-bind`表达式中。

以下是一个简单的例子，展示了如何在Vue中使用过滤器：

```html
<div id="app">
  <p>{{ message | capitalize }}</p>
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    message: 'hello world'
  },
  filters: {
    capitalize: function (value) {
      if (!value) return '';
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1);
    }
  }
})
```

在上述例子中，`capitalize` 过滤器用于将 `message` 的内容首字母大写显示。

你也可以通过在 `v-bind` 中使用过滤器，比如：

```html
<img :src="imageUrl | addBaseUrl">
```

```javascript
new Vue({
  data: {
    imageUrl: 'example.jpg'
  },
  filters: {
    addBaseUrl: function (value) {
      if (!value) return '';
      return 'https://example.com/' + value;
    }
  }
})
```

这个例子中，`addBaseUrl` 过滤器用于将图片URL与基础URL连接起来。

Vue提供了很多内置的过滤器，如 `uppercase`、`lowercase`、`currency`等。你还可以自定义过滤器来满足特定的需求。要定义全局过滤器，可以使用 `Vue.filter` 方法：

```javascript
Vue.filter('reverse', function (value) {
  if (!value) return '';
  return value.split('').reverse().join('');
});
```

然后你可以在整个应用程序中使用 `reverse` 过滤器。希望这能帮助你理解Vue中过滤器的基本用法。



### Props 

在 Vue.js 中，`Props`（属性）是用于父组件向子组件传递数据的一种机制。它允许父组件向子组件传递数据，并且这些数据是单向流动的，子组件接收到的 `props` 数据不应该被子组件直接修改。

在JavaScript中定义函数时无须指定参数的类型，但并不是特别安全
Vue在定义组件的Props时，可以通过添加约束的方式来对其类型、默认值、是否选填等进行配置。

从功能上讲，Props也可以称为组件的外部属性，通过Props的不同传参，组件可以有很强的灵活性和扩展性。
父组件可以通过Props将数据传递给子组件，子组件通过Props选项接收数据。


![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028071.jpeg)

上面的代码中定义了一个名为count的外部属性，这个属性在组件内实际上的作用是控制组件计数的初始值。需要注意，在外部传递数值类型的数据到组件内部时，**必须使用v-bind指令的方式进行传递**，直接使用HTML属性设置的方式会将传递的数据作为字符串传递（而不是JavaScript表达式）。

将其配置为对象，则可以进行更多约束设置。修改上面代码中props的定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024137.jpeg)

在调用此组件时，如果设置count属性的值不符合要求，则控制台会输出警告信息，例如count设置的值不是数值类型，则会抛出如下警告：

```
    [Vue warn]: Invalid prop: type check failed for prop "count". Expected Number
with value NaN, got Object
```

在实际开发中，我们建议所有的props都采用对象的方式定义，显式地设置其类型、默认值等，这样不仅可以使组件调用时更加安全，也侧面为开发者提供了组件的参数使用文档。

如果只需要指定属性的类型，而不需要指定更加复杂的性质，可以使用如下方式定义：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112028082.jpeg)

如果一个属性可能是多种类型，可以如下定义：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030476.jpeg)

在对属性的默认值进行配置时，如果默认值的获取方式比较复杂，也可以将其定义为函数，函数执行的结果会被当作当前属性的默认值，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024035.jpeg)

Vue中props的定义也支持进行自定义的验证，以上面的代码为例，假设组件内需要接收的count属性的值必须大于数值10，则可以通过自定义验证函数实现：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029333.jpeg)

当组件的count属性被赋值时，会自动调用验证函数进行验证，如果验证函数返回true，则表明此赋值是有效的，如果验证函数返回false，则控制台会输出异常信息。



对组件内部来说，Props是只读的。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024319.jpeg)

当click函数被触发时，页面上的计数并没有改变，并且控制台会抛出Vue警告信息。

Props的这种只读性能是Vue单向数据流特性的一种体现。所有的外部属性Props都只允许父组件的数据流动到子组件中，子组件的数据则不允许流向父组件。因此，在组件内部修改Props的值是无效的。







### 进行函数限流

在工程开发中，限流是一个非常重要的概念。我们在实际开发中也经常会遇到需要进行限流的场景，例如网页上的某个按钮当用户单击后会从后端服务器进行数据的请求，在数据请求回来之前，用户额外的单击是无效的且消耗性能的。或者，网页中某个按钮会导致页面的更新，我们需要限制用户对其频繁地进行操作。这时就可以使用限流函数，常见的限流方案是根据时间间隔进行限流，即在指定的时间间隔内不允许重复执行同一函数。

本节将讨论如何在前端开发中使用限流函数。

#### 3.3.1　手动实现一个简易的限流函数

我们先来尝试手动实现一个基于时间间隔的限流函数，要实现这样一个功能：页面中有一个按钮，单击按钮后通过打印方法在控制台输出当前的时间，要求这个按钮的两次事件触发间隔不能小于2秒。

新建一个名为throttle.html的测试文件，分析我们需要实现的功能，直接的思路是使用一个变量来控制按钮事件是否可触发，在触发按钮事件时对此变量进行修改，并使用setTimeout函数来控制2秒后将变量的值还原。使用这个思路来实现限流函数非常简单，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024210.jpeg)

运行上面的代码，快速单击页面上的按钮，从VSCode控制台可以看到，无论按钮被单击了多少次，打印方法都按照每2秒最多执行1次的频率进行限流。其实，在上述示例代码中，限流本身是一种通用的逻辑，打印时间才是业务逻辑，因此我们可以将限流的逻辑封装成单独的工具方法，修改核心JavaScript代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024281.jpeg)

再次运行代码，程序依然可以正确地运行。现在我们已经有了一个限流工具，可以为任意函数增加限流功能，并且可以任意地设置限流的时间间隔。

#### 3.3.2　使用Lodash库进行函数限流

目前我们已经了解了限流函数的实现逻辑，在3.3.1节中也手动实现了一个简单的限流工具，尽管其能够满足当前的需求，细细分析，还有许多需要优化的地方。在实际开发中，每个业务函数需要的限流间隔都不同，而且需要各自独立地进行限流，我们自己编写的限流工具就无法满足了，但是得益于JavaScript生态的繁荣，有许多第三方工具库都提供了函数限流功能，它们强大且易用，Lodash库就是其中之一。

Lodash是一款高性能的JavaScript实用工具库，其提供了大量的数组、对象、字符串等边界的操作方法，使开发者可以更加简单地使用JavaScript来编程。

Lodash库中提供了debounce函数来进行方法的调用限流，要使用它，首先需要引入Lodash库，代码如下：

```
     <script src="https://unpkg.com/lodash@4.17.20/lodash.min.js"></script>
```

以3.3.1节编写的代码为例，修改代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029646.jpeg)

运行代码，体验一下Lodash限流函数的功能。



### Mixin

混入对象，混入对象中可以包含任意的组件定义选项，当此对象被混入组件时，组件会将混入对象中提供的选项引入当前组件内部。这有些类似于编程语言中“继承”的语法。

#### Mixin选项的合并

当混入对象与组件中定义了相同的选项时，Vue可以非常智能地对这些选项进行合并。不冲突的配置将完整合并，冲突的配置会以组件中自己的配置为准

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029575.jpeg)

不重名的生命周期函数会被完整混入组件，重名的生命周期函数被混入组件时，
在函数触发时，会先触发Mixin对象中的实现，再触发组件内部的实现，这类似于面向对象编程中子类对父类方法的覆写，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024019.jpeg)

运行上面的代码，当com组件被挂载时，控制台会先打印“Mixin对象mounted”之后再打印“组件本身mounted”。

#### 全局Mixin

Vue也支持对应用进行全局Mixin混入。直接对应用实例进行Mixin设置即可，实例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024495.jpeg)

需要注意，虽然全局Mixin使用起来非常方便，但是其会使之后所有注册的组件都默认被混入这些选项，当程序出现问题时，这会增加排查问题的难度。全局Mixin技术非常适合开发插件，如开发组件挂载的记录工具等。



### Teleport

翻译为“传送，传递”，是Vue 3.0提供的新功能。有了Teleport功能，在编写代码时，开发者可以将相关行为的逻辑和UI封装到同一个组件中，以提高代码的聚合性。

在定义组件时，如果组件模板中的某些元素只能挂载在指定的标签下，可以使用Teleport来指定，可以形象地理解Teleport的功能是将此部分元素“传送”到指令的标签下，以上面的代码为例，可以指定全局弹窗只挂载在body元素下，修改如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029966.jpeg)

优化后的代码无论组件本身在组件树中的何处，弹窗都能正确地布局。



### 插槽（Slots）

插槽用于在组件内部插入内容，允许父组件向子组件传递内容。插槽可用于构建可配置的组件，使其能够包含不同的内容。

Vue.js中的插槽（Slot）是一种非常强大和灵活的机制，用于在父组件中定义可插入子组件的内容。插槽允许你在子组件中定义一些占位符，然后在父组件中填充这些占位符，以动态地传递内容。Vue提供了两种类型的插槽：具名插槽和作用域插槽。

### 具名插槽

具名插槽允许你在父组件中选择性地分发内容到子组件的不同插槽中。在子组件中，通过`<slot>`标签定义具名插槽。

**子组件（ChildComponent.vue）：**

```html
<template>
  <div>
    <h2>子组件</h2>
    <slot name="header"></slot>
    <div>其他内容</div>
    <slot name="footer"></slot>
  </div>
</template>
```

**父组件（ParentComponent.vue）：**

```html
<template>
  <div>
    <child-component>
      <template v-slot:header>
        <h3>父组件提供的标题</h3>
      </template>
      <template v-slot:footer>
        <p>父组件提供的页脚</p>
      </template>
    </child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
}
</script>
```

在父组件中，通过`v-slot`指令和插槽的名称，将内容分发到子组件的不同插槽中。

### 作用域插槽

作用域插槽允许子组件向父组件传递数据。在子组件中，通过`<slot>`标签的`v-bind`属性将数据传递给插槽。在父组件中，可以使用带有`v-slot`指令的`template`元素接收这些数据。

**子组件（ChildComponent.vue）：**

```html
<template>
  <div>
    <h2>子组件</h2>
    <slot :message="message"></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello from child!'
    };
  }
}
</script>
```

**父组件（ParentComponent.vue）：**

```html
<template>
  <div>
    <child-component>
      <template v-slot="slotProps">
        <p>{{ slotProps.message }}</p>
      </template>
    </child-component>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
}
</script>
```

在这个例子中，`slotProps`是一个包含插槽中数据的对象，通过它可以在父组件中访问子组件的数据。

Vue插槽是Vue组件中非常强大和灵活的特性，能够有效地实现组件之间的通信和内容的动态分发。

### 过渡和动画

（Transitions and Animations）

Vue.js支持在组件进入、离开或更新时应用过渡和动画效果。通过`<transition>`和`<transition-group>`元素，您可以实现这些效果。

------



## 生命周期

组件从创建到销毁的这一系列过程被称为组件的生命周期。在Vue中，组件生命周期的节点会被定义为一系列的方法，这些方法称为生命周期钩子。

在Vue.js中，每个Vue实例都有一个生命周期，它表示了Vue实例在创建、更新和销毁时经历的不同阶段。Vue生命周期的理解对于开发和调试Vue应用程序非常重要，因为它允许您在不同的时机执行自定义逻辑。Vue生命周期钩子是一些特定的方法，它们会在不同的生命周期阶段被自动调用。以下是Vue.js中的主要生命周期钩子：

1. **`beforeCreate`**：在Vue实例被创建之前立即调用。此时，Vue实例的数据和事件还没有初始化，无法访问`data`或`methods`。

2. **`created`**：在Vue实例被创建之后立即调用。此时，Vue实例已经初始化，可以访问`data`和`methods`，但尚未挂载到DOM。

3. **`beforeMount`**：在Vue实例挂载到DOM之前调用。此时，Vue实例的模板编译已经完成，但尚未替换真实DOM。

4. **`mounted`**：在Vue实例挂载到DOM之后调用。此时，Vue实例已经挂载到DOM，可以访问DOM元素。

5. **`beforeUpdate`**：在数据发生变化之前（例如，通过`v-model`或`v-on`指令），在更新之前调用。此时，DOM尚未更新。

6. **`updated`**：在数据发生变化之后，Vue实例更新DOM后调用。此时，可以访问更新后的DOM。

7. **`beforeDestroy`**：在Vue实例销毁之前调用。可以用来清理定时器、取消订阅等资源。

8. **`destroyed`**：在Vue实例销毁之后调用。此时，Vue实例已经从DOM中移除，不再可用。

这些生命周期钩子可以用于执行各种任务，例如数据初始化、异步请求、DOM操作、事件监听、资源清理等。您可以在Vue组件的选项中定义这些生命周期钩子，以便在特定的生命周期阶段执行自定义逻辑。以下是一个示例：

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello, Vue!"
    };
  },
  created() {
    console.log("Vue instance created");
  },
  updated() {
    console.log("Data updated");
  },
  beforeDestroy() {
    console.log("Vue instance will be destroyed");
  },
  destroyed() {
    console.log("Vue instance destroyed");
  },
  methods: {
    updateMessage() {
      this.message = "New message";
    }
  }
}
</script>
```

在上述示例中，我们定义了`created`、`updated`、`beforeDestroy`和`destroyed`生命周期钩子，并在每个钩子中输出了相应的日志信息。这有助于理解Vue实例在不同阶段的行为。生命周期钩子是Vue.js中强大的工具，可用于执行各种任务和响应应用程序的状态变化。


<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024233.jpeg" alt="img" style="zoom:50%;" />

如果某个组件是通过v-if指令来控制其渲染的，则当其渲染状态切换时，组件会进行交替的挂载和卸载动作

当单击页面中的按钮时，页面显示的计数会自增，同时控制台打印信息如下：

```vue
     虚拟DOM被触发渲染时调用
     组件即将更新前
     虚拟DOM重新渲染时调用
     组件更新完成
```









## 数据传递



###  父—>子

1. **在父组件中传递数据给子组件：**

父组件可以使用 `v-bind` 指令将数据作为属性绑定到子组件上，并在子组件中定义相应的 `props` 接收这些数据。

```vue
<!-- 父组件模板 -->
<template>
    <!-- 在父组件中使用 ChildComponent 作为子组件  拼写方式不一样  大驼峰命名法--》横线 -->
  <child-component :message="parentMessage"></child-component> <!--子组件-->
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      parentMessage: 'Hello from parent!'
    };
  }
};
</script>
```

2. **子组件中接收 Props 数据：**

在子组件中，使用 `props` 选项来声明需要接收的属性，并且可以在模板中使用这些属性的值。

```vue
<!-- 子组件模板 -->
<template>
  <div>
    <p>Message from parent: {{ message }}</p>
  </div>
</template>

<script>
export default {
  props: ['message'] // 声明接收的props
};
</script>
```

在这个例子中，`message` 是父组件传递给子组件的一个属性，子组件可以通过 `props` 来接收并在自己的模板中使用这个数据。

注意事项：

- Props 是单向数据流，子组件不能直接修改父组件传递的 Props 数据。
- Props 是响应式的，如果父组件的数据发生变化，传递给子组件的 Props 也会随之更新。
- 子组件可以通过 `props` 接收任何数据类型的属性，并且可以在声明中指定属性的类型、默认值等。

`Props` 在 Vue 中是一种非常重要的数据传递机制，它使得组件之间的通信变得简单而直观。通过 Props，你可以将数据从父组件传递到子组件，从而构建出更加灵活和可复用的组件。





###  子—>父  **自定义事件** 

`this.$emit` 是Vue实例中用于触发自定义事件的方法。通过该方法，子组件可以向父组件传递数据，实现组件之间的通信。

组件可以触发自定义事件，允许父组件监听这些事件并执行相应的操作。这是组件之间通信的一种方式。

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <my-component @child-event="handleChildEvent" />
  </div>
</template>

<script>
import MyComponent from './MyComponent.vue';

export default {
  components: {
    'my-component': MyComponent
  },
  data() {
    return {
      message: "Hello from parent"
    };
  },
  methods: {
    handleChildEvent(data) {
      console.log("Data received from child:", data);
    }
  }
}
</script>
```

子组件

```vue
<template>
  <button @click="sendDataToParent">Send Data to Parent</button>
</template>

<script>
export default {
  methods: {
    sendDataToParent() {
      this.$emit("child-event", "Data from child");
    },
  },
};
</script>

```



### **Provide/Inject **  数据注入  

Provide 和 Inject 允许您在祖先组件中提供数据，并在后代组件中注入它。这对于跨多个层次的组件传递数据非常有用。

在提供数据的祖先组件中：

```vue
<script>
export default {
  provide() {
    return {
      dataToInject: "Data provided by ancestor",
    };
  },
};
</script>
```

在注入数据的后代组件中：

```vue
<template>
  <div>{{ dataToInject }}</div>
</template>

<script>
export default {
  inject: ["dataToInject"],
};
</script>
```





# 动画

## 使用CSS3创建动画

CSS3本身支持非常丰富的动画效果。组件的过渡、渐变、移动、翻转等都可以添加动画效果。CSS3动画的核心是定义keyframes或transition。keyframes定义了动画的行为，比如对于颜色渐变的动画，需要定义起始颜色和终止颜色，浏览器会自动帮助我们计算其间的所有中间态来执行动画。transition的使用更加简单，当组件的CSS属性发生变化时，使用transition来定义其过渡动画的属性即可。

### transition过渡动画

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024615.jpeg)

如以上代码所示，CSS中定义的demo-ani类中指定了transition属性，这个属性中可以设置要过渡的属性以及动画时间。运行上面的代码，单击页面中的色块，可以看到色块变大的过程会附带动画效果，颜色变化的过程也附带动画效果。上面的示例代码实际上使用了简写方式，我们也可以逐条属性对动画效果进行设置。示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024678.jpeg)

其中，transition-property用来设置动画的属性；transition-duration用来设置动画的执行时长；transition-timing-function用来设置动画的执行方式，linear表示以线性的方式执行；transition-delay用来进行延时设置，即延时多长时间后开始执行动画。

### keyframes动画

transition动画适合用来创建简单的过渡效果。CSS3中支持使用animation属性来配置更加复杂的动画效果。animation属性根据keyframes配置来执行基于关键帧的动画效果。新建一个名为keyframes.html的测试文件。编写如下测试代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024458.jpeg)

在上面的CSS代码中，keyframes用来定义动画的名称和每个关键帧的状态，0%表示动画起始时的状态，25%表示动画执行到1/4时的状态，同理，100%表示动画的终止状态。对于每个状态，我们将其定义为一个关键帧，在关键帧中，可以定义元素的各种渲染属性，比如宽和高、位置、颜色等。在定义keyframes时，如果只关心起始状态与终止状态，也可以这样定义：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029508.jpeg)

定义好了keyframes关键帧，在编写CSS样式代码时可以使用animation属性为其指定动画效果，如以上代码设置要执行的动画为名为animation1的关键帧动画，执行时长为4秒，执行方式为线性。animation的这些配置项也可以分别进行设置，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024085.jpeg)

通过上面的范例，我们已经基本了解了如何使用原生的CSS，有了这些基础，再使用Vue中提供的动画相关API时会非常容易。

## 使用JavaScript的方式实现动画效果

动画的本质是将元素的变化以渐进的方式完成，即将大的状态变化拆分成非常多个小的状态变化，通过不断执行这些变化来达到动画的效果。使用JavaScript代码来启用定时器，按照一定频率进行组件状态的变化也可以实现动画效果。下面我们来看一个JavaScript动画示例。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024973.jpeg)

setInterval方法用来开启一个定时器，上面代码中设置每10毫秒执行一次回调函数，在回调函数中，我们逐像素地将色块的尺寸放大，最终就产生了动画效果。使用JavaScript可以更加灵活地控制动画的效果，在实际开发中，结合Canvas的使用，JavaScript可以实现非常强大的自定义动画效果。还有一点需要注意，当动画结束后，要使用clearInterval方法将对应的定时器停止。



## Vue过渡动画



Vue组件在页面中被插入、移除或者更新的时候都可以附带转场效果，即可以展示过渡动画。例如，当我们使用v-if和v-show指令控制组件的显示和隐藏时，就可以将其过程以动画的方式进行展现。

#### 8.3.1　定义过渡动画

Vue过渡动画的核心原理依然是采用CSS类来实现的，只是Vue帮助我们在组件的不同生命周期自动切换不同的CSS类。

Vue中默认提供了一个名为transition的内置组件，可以用其来包装要展示过渡动画的组件。transition组件的name属性用来设置要执行的动画名称，Vue中约定了一系列CSS类名规则来定义各个过渡过程中的组件状态。我们可以通过一个简单的示例来体会Vue的这一功能。

首先，新建一个名为vueTransition.html的测试文件，编写如下示例代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029875.jpeg)

运行代码，尝试单击页面上的功能按钮，可以看到组件在显示／隐藏过程中表现出的过渡动画效果。上面代码的核心是定义的6个特殊的CSS类，这6个CSS类没有显式地使用，但是其在组件执行动画的过程中起到了不可替代的作用。当我们为transition组件的name属性设置动画名称之后，当组件被插入页面被移除时，其会自动寻找以此动画名称开头的CSS类，格式如下：

```
     x-enter-from
     x-enter-active
     x-enter-to
     x-leave-from
     x-leave-active
     x-leave-to
```

其中，x表示定义的过渡动画名称。上面6种特殊的CSS类，前3种用来定义组件被插入页面的动画效果，后3种用来定义组件被移出页面的动画效果。

x-enter-from类在组件即将被插入页面时被添加到组件上，可以理解为组件的初始状态，元素被插入页面后此类会马上被移除。

v-enter-to类在组件被插入页面后立即被添加，此时x-enter-from类会被移除，可以理解为组件过渡的最终状态。

v-enter-active类在组件的整个插入过渡动画中都会被添加，直到组件的过渡动画结束后才会被移除。可以在这个类中定义组件过渡动画的时长、方式、延迟等。

x-leave-from与x-enter-from相对应，在组件即将被移除时此类会被添加。用来定义移除组件时过渡动画的起始状态。

x-leave-to则对应地用来设置移除组件动画的终止状态。

x-leave-active类在组件的整个移除过渡动画中都会被添加，直到组件的过渡动画结束后才会被移除。可以在这个类中定义组件过渡动画的时长、方式、延迟等。

你可能也发现了，上面提到的6种特殊的CSS类虽然被添加的时机不同，但是**最终都会被移除**，因此当动画执行完成后，组件的样式并不会保留，**更常见的做法是在组件本身绑定一个最终状态的样式类**，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029945.jpeg)

CSS代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024674.jpeg)

这样，组件的显示或隐藏过程就变得非常流畅。上面的示例代码使用CSS中的transition来实现动画，其实使用animation的关键帧方式定义动画效果也是一样的，CSS示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029979.jpeg)

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024205.jpeg)

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024818.jpeg)

#### 8.3.2　设置动画过程中的监听回调

我们知道，对于组件的加载或卸载过程，有一系列的生命周期函数会被调用。对于Vue中的转场动画来说，也可以注册一系列的函数来对其过程进行监听。示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024936.jpeg)

上面注册的回调方法需要在组件的methods选项中实现：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029871.jpeg)

有了这些回调函数，可以在组件过渡动画过程中实现复杂的业务逻辑，也可以通过JavaScript来自定义过渡动画，当我们需要自定义过渡动画时，需要将transition组件的CSS属性关掉，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024970.jpeg)

还有一点需要注意，上面列举的回调函数中，有两个函数比较特殊：enter和leave。这两个函数除了会将当前元素作为参数外，还有一个函数类型的done参数，如果我们将transition组件的CSS属性关闭，决定使用JavaScript来实现自定义的过渡动画，这两个方法中的done函数最后必须被手动调用，否则过渡动画会立即完成。

#### 8.3.3　多个组件的过渡动画

Vue中的transition组件支持同时包装多个互斥的子组件元素，从而实现多组件的过渡效果。在实际开发中，有很多这类常见的场景，例如元素A消失的同时元素B展示。核心示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029914.jpeg)

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029206.jpeg)

运行代码，单击页面上的按钮，可以看到两个色块会以过渡动画的方式交替出现。默认情况下，两个元素的插入和移除动画会同步进行，有些时候这并不能满足我们的需求，大多数时候需要移除动画执行完成后，再执行插入动画。要实现这一功能非常简单，只需要对transition组件的mode属性进行设置即可，当我们将其设置为out-in时，就会先执行移除动画，再执行插入动画；将其设置为in-out时，则会先执行插入动画，再执行移除动画，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024216.jpeg)

#### 8.3.4　列表过渡动画

在实际开发中，列表是一种非常流行的页面设计方式。在Vue中，通常使用v-for指令来动态构建列表视图。在动态构建列表视图的过程中，其中的元素经常会有增删、重排等操作，在Vue中使用transition-group组件可以非常方便地实现列表元素变动的动画效果。

新建一个名为listAnimation.html的测试文件，编写如下核心示例代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024899.jpeg)

上面的代码非常简单，可以尝试运行一下，单击页面中的“添加元素”按钮后，可以看到列表的元素在增加，并且是以渐进动画的方式插入的。

在使用transition-group组件实现列表动画时，与transition类似，首先需要定义动画所需的CSS类，上面的示例代码中，我们只定义了透明度变化的动画。有一点需要注意，如果要使用列表动画，列表中的每一个元素都需要有一个唯一的key值。如果为上面的列表再添加一个删除元素的功能，它依然会很好地展示动画效果，删除元素的方法如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029548.jpeg)

除了对列表中的元素进行插入和删除可以添加动画外，对列表元素的排序过程也可以采用动画来进行过渡，只需要额外定义一个v-move类型的特殊动画类即可，例如为上面的代码增加如下CSS类：

```
     .list-move {
         transition: transform 1s ease;
     }
```

之后可以尝试对列表中的元素进行逆序，Vue会以动画的方式将其中的元素移动到正确的位置。

------





# 开发与构建工具

## 比较

Vite：

- **特点：** Vite 是一个面向现代浏览器的构建工具，专注于快速开发。它利用原生 ES 模块的特性，采用了基于 ES Module 的开发模式，能够在开发阶段实现快速冷启动，提供了极致的开发体验。
- **优点：** 快速的热更新、开箱即用的模块热更新、快速的 HMR（模块热替换）、按需编译，支持 Vue 单文件组件、快速的构建速度。
- **适用场景：** 适用于需要快速开发、更快的热更新和优化构建速度的项目。

Vue CLI：

- **特点：** Vue CLI 是官方提供的 Vue.js 项目脚手架，内置了对 Vue 项目的开发所需的一切工具和配置，包括构建、打包、测试等等，提供了更多的插件和预设选项。
- **优点：** 完善的生态系统、内置了很多常用的功能和插件、更适合大型项目和生产环境、更多的配置选项。
- **适用场景：** 适用于需要更多自定义和配置、大型项目、生产环境等。

如何选择：

- **开发速度 vs. 生态和定制：** 如果你需要快速开发和热更新，那么 Vite 可能更适合；如果需要更多的配置和功能、以及更成熟的生态系统，那么 Vue CLI 可能更适合。
- **项目规模：** 对于小型项目和快速原型开发，Vite 可能更合适；对于大型项目和需要更多配置和插件的情况，Vue CLI 可能更适合。

综合来说，两者并非互斥，而是针对不同的需求和场景提供了不同的解决方案。根据具体项目的需求和团队的实际情况进行选择。在实际项目中，也可以根据需要在不同的场景下灵活切换使用。



## VueCLI



### Webpack 

是一个模块打包工具，它主要用于将各种资源，如 JavaScript、CSS、图片等，视为模块，并将这些模块打包成静态文件以用于浏览器。Webpack 提供了丰富的功能和配置选项，可以帮助开发者更高效地管理前端项目的依赖和资源。

主要特点和功能：

1. **模块化打包：** Webpack 将项目中的各种文件（JS、CSS、图片等）视为模块，并通过模块系统进行处理和打包。

2. **Loader：** 允许你在打包过程中对不同类型的文件应用各种转换（比如将 ES6 转为 ES5，将 Sass 编译为 CSS 等）。

3. **Plugin：** 丰富的插件系统，提供了各种功能的扩展和优化，比如代码压缩、打包分析、环境变量注入等。

4. **代码分割和懒加载：** 支持将代码分割成多个文件，实现懒加载和按需加载，提高应用性能。

5. **Hot Module Replacement（HMR）：** 支持模块热替换，能够在应用运行时实现无需刷新页面的更新。

基本概念：

- **Entry：** 指定 Webpack 的入口文件，Webpack 从这里开始构建依赖图。

- **Output：** 指定 Webpack 打包后的输出目录和文件名等配置。

- **Loaders：** 用于处理各种类型文件的转换器，例如将 ES6 转为 ES5、将 SCSS 转为 CSS 等。

- **Plugins：** 用于扩展 Webpack 功能的插件，例如压缩代码、提取 CSS、注入环境变量等。

- **Chunks 和 Code Splitting：** 可以将代码分割成多个文件，提高性能和加载速度。

Webpack 配置文件示例：

一个简单的 Webpack 配置文件可能如下所示：

```javascript
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: 'babel-loader'
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  plugins: [
    // 插件配置
  ]
};
```

以上是一个简单的 Webpack 配置文件示例，定义了入口文件、输出文件、Loader 规则和插件配置等。根据项目需求和具体情况，Webpack 配置文件可能会更加复杂和丰富。

#### Vue CLI的安装

Vue CLI是一个需要全局安装的NPM包，安装Vue CLI的前提是设备已经安装了Node.js环境，如果你使用的是macOS操作系统，则系统默认会安装Node.js环境。如果系统默认没有安装，手动进行安装也非常简单。访问Node.js官网（https://nodejs.org），网页打开后，在页面中间直接可以看到一个Node.js软件下载入口，如图9-1所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024087.jpeg)

图9-1　Node.js官网

Node.js官网会自动根据当前设备的系统类型推荐需要下载的软件，选择当前新的稳定版本进行下载即可。下载完成后，按照安装普通软件的方式来对其进行安装。

安装Node.js环境后，即可在终端使用npm相关指令来安装软件包。在终端输入如下命令可以检查Node.js环境是否正确安装完成：

```
     node -v
```

执行上面的命令后，只要终端输出了版本号信息，就表明Node.js已经安装成功。

下面使用npm安装Vue CLI工具。在终端输入如下命令并执行：

```
     npm install -g @vue/cli
```

由于有很多依赖包需要下载，因此安装过程可能会持续一段时间，耐心等待即可。需要注意，如果在安装过程中终端输出了如下异常错误信息：

```
     Unhandled rejection Error: EACCES: permission denied
```

这是因为当前操作系统登录的用户权限不足，使用如下命令重新安装即可：

```
     sudo npm install -g @vue/cli
```

在命令前面添加sudo表示使用超级管理员权限进行命令的执行，执行命令前终端会要求输入设备的启动密码。等待终端安装完成后，可以使用如下命令检查Vue CLI工具是否安装成功：

```
     vue --version
```

如果终端正确输出了工具的版本号，则表明已经安装成功。之后，如果官方的Vue CLI工具有升级，在终端使用如下命令即可进行升级：

```
     npm update -g @vue/cli
```

#### 快速创建项目

本节将演示使用Vue CLI创建一个完整的Vue项目工程的过程。在终端执行如下命令来创建Vue项目工程：

```
     vue create hello-world
```

其中hello-world是我们要创建的工程名称，Vue CLI工具本身是有交互性的，执行上面的命令后，终端会输出如下信息询问我们是否需要替换资源地址：

```
     Your connection to the default yarn registry seems to be slow.
     Use https://registry.npm.taobao.org for faster installation?
```

输入Y表示同意，之后继续创建工程，之后终端还会询问一系列的配置问题，都选择默认的选项即可。完成所有的初始配置工作后，稍等片刻，Vue CLI即可创建一个Vue项目模板工程。打开此工程目录，可以看到当前工程的目录结构如图9-2所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112029103.jpeg)

图9-2　Vue模板工程的目录结构

一个完整的Vue模板工程相对原生的HTML工程要复杂很多，后面我们会介绍工程中默认生成的文件夹及文件的意义和用法。

至此，我们已经使用Vue CLI工具创建了第一个完整的Vue项目工程，上面是使用终端交互式命令的方式创建工程的，Vue CLI工具也可以使用可交互的图形化页面创建工程。在终端输入如下命令即可在浏览器中打开一个Vue工程管理工具页面：

```
     vue ui
```

初始页面如图9-3所示。

可以看到，在页面中可以创建项目、导入项目或对已经有的项目进行管理。现在，我们可以尝试创建一个项目，单击创建项目按钮，之后需要对项目的详情进行完善，如图9-4所示。

在详情设置页面中，需要填写项目的名称，选择项目所在的目录位置，选择项目包管理器以及进行Git等相关配置。完成后，进行下一步选择项目的预设，如图9-5所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030322.jpeg)

图9-3　Vue CLI图形化工具页面

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030340.jpeg)

图9-4　对所创建的项目详情进行设置

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024698.jpeg)

图9-5　选择项目预设

可以选择Default preset(Vue 3)这套项目预设。之后单击“创建项目”按钮，页面即可进入项目创建过程，需要稍等片刻，创建完成后可以进入对应的目录查看，使用图形化页面创建的项目与使用终端命令创建的项目结构是一样的。

使用命令的方式创建的管理项目与使用图形化页面的方式创建的管理项目的功能是一样的，可以根据自己的习惯来进行选择。总体来说，使用命令的方式更加便捷，而使用图形化页面的方式更加直观。

### Vue CLI项目模板工程

前面我们使用Vue CLI工具创建了一个完整的Vue项目工程。其实项目工程的创建只是Vue CLI工具链中的一部分，在安装Vue CLI工具时，也同步安装了vue-cli-service工具，其提供了Vue项目的代码检查、编译、服务部署等功能。本节将向读者介绍Vue CLI创建的模板工程的目录结构，并对这个模板工程进行运行。

#### 模板工程的目录结构

通过观察Vue CLI创建的工程目录，可以发现其中主要包含3个文件夹和5个独立文件。我们先来看这5个独立文件：

·　.gitignore文件。

·　babel.config.js文件。

·　package.json文件。

·　README.md文件。

·　yarn.lock文件。

.gitignore是一个隐藏文件，用来配置Git版本管理工具需要忽略的文件或文件夹，在创建工程时，其默认会配置好，将一些依赖、编译产物、log日志等文件忽略，我们不需要修改。

babel.config.js是Babel工具的配置文件，Babel本身是一个JavaScript编译器，其会将ES 6版本的代码转换成向后兼容的JavaScript代码，这个文件我们一般也无须修改。

package.json是一个相对比较重要的文件，其中存储的是一个JSON对象数据，用来配置当前的项目名称、版本号、脚本命令以及模块依赖等。当我们需要向项目中添加额外的依赖时，其就会被记录到这个文件默认的模板工程中，生产环境的依赖如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024509.jpeg)

开发环境的依赖如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030018.jpeg)

README.md是一个MarkDown格式的文件，其中记录了项目的编译和调试方式。我们也可以将项目的介绍编写在这个文件中。

最后，yarn.lock文件记录了YARN包管理器安装的依赖版本信息，我们无须关心。

了解了这些独立文件的意义及用法，再来看一下默认生成的3个文件夹：

·　node_modules。

·　public。

·　src。

其中，node_modules文件下存放的是NPM安装的依赖模块，这个文件夹默认会被Git版本管理工具忽略，对于其中的文件，我们也不需要手动添加或修改。

public文件夹正如其命名一样，用来放置一些公有的资源文件，例如网页用到的图标、静态的HTML文件等。

src是一个重要的文件夹，核心功能代码文件都放在这个文件夹下，在默认的模板工程中，这个文件夹下还有两个子文件夹：assets和components。顾名思义，assets存放资源文件，components存放组件文件。我们按照页面的加载流程来看一下src文件下默认生成的几个文件。

main.js是应用程序的入口文件，其中的代码如下：

```
     // 导入vue框架中的createApp方法
     import { createApp } from 'vue'
     // 导入我们自定义的根组件
     import App from './App.vue'
     // 挂载根组件
     createApp(App).mount('#app')
```

你可能有些疑惑，main.js文件中怎么只有组件创建和挂载的相关逻辑，并没有对应的HTML代码，那么组件是挂载到哪里的呢？其实前面我们已经介绍过了，在public文件夹下会包含一个名为index.html的文件，它就是网页的入口文件，其中的代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024609.jpeg)

现在你明白了吧，main.js中定义的根组件将被挂载到id为“app”的div标签上。回到main.js文件，其中导入了一个名为App的组件作为根组件，可以看到，项目工程中有一个名为App.vue的文件，这其实使用了Vue中单文件组件的定义方法，即将组件定义在单独的文件中，以便于开发和维护。

App.vue文件中的内容如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024780.jpeg)

单文件组件通常需要定义3部分内容：template模板部分、script脚本代码部分和style样式代码部分。如以上代码所示，在template模板中布局了一个图标和一个自定义的HelloWorld组件，在JavaScript中将当前组件进行了导出。下面我们再来关注一下HelloWorld.vue文件，其中的内容如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030183.jpeg)

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112024957.jpeg)

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025321.jpeg)

HelloWorld.vue文件中的代码很多，总体来看只是定义了很多可跳转元素，并没有太多逻辑。现在我们对默认生成的模板项目已经有了初步的了解，下一节将尝试在本地运行和调试它。

#### 运行Vue项目工程

要运行Vue模板项目非常简单，首先打开终端，进入当前Vue项目的工程目录，执行如下命令即可：

```
     npm run serve
```

之后，进行Vue项目工程的编译，并在本机启动一个开发服务器，若终端输出如下信息，则表明项目已经运行完成：

```
     DONE  Compiled successfully in 3168ms               7:29:53 PM
      App running at:
      - Local:   http://localhost:8080/
      - Network: http://192.168.26.96:8080/
      Note that the development build is not optimized.
      To create a production build, run yarn build.
```

之后，在浏览器中输入如下地址，便会打开当前的Vue项目页面，如图9-6所示。

```
     http://localhost:8080/
```

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025626.jpeg)

图9-6　HelloWorld示例项目

默认情况下，Vue项目要运行在8080端口上。我们也可以手动指定端口，示例如下：

```
     npm run serve -- --port 9000
```

当启动了开发服务器后，其默认附带了热重载模块，即我们只需要在修改代码之后进行保存，网页就会自动进行更新。你可以尝试修改App.vue文件中HelloWorld组件的msg参数，之后保存，可以看到浏览器页面中的标题也会自动进行更新。

使用Vue CLI中的图形化页面可以更加方便和直观地对Vue项目进行编译和运行，从CLI图形化网页工具中进入对应的项目，单击页面中的“运行”按钮即可，如图9-7所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025655.jpeg)

图9-7　使用图形化工具管理Vue项目

在图形化工具中不仅可以对项目进行编译、运行和调试，还提供了许多分析报表，比如资源体积、运行速度、依赖项等，非常好用。

### 在项目中使用依赖

在Vue项目开发中，额外插件的使用必不可少。后面还会给读者介绍各种各样的常用Vue插件，如网络插件、路由插件、状态管理插件等。本节将介绍如何使用Vue CLI工程脚手架来安装和管理插件。

Vue CLI创建的工程使用的是基于插件的架构。通过查看package.json文件，可以发现在开发环境下，其默认安装了需要的工具依赖，主要用来进行代码编译、服务运行和代码检查等。安装依赖包依然使用npm相关命令。例如，如果需要安装vue-axios依赖，可以在项目工程目录下执行如下命令：

```
     npm install --save axios vue-axios
```

需要注意，如果安装过程中出现权限问题，则需要在命令前添加sudo再执行。

安装完成后，可以看到package.json文件会自动进行更新，更新后的依赖信息如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030313.jpeg)

其实，不止package.json文件会更新，在node_modules文件夹下也会新增axios和vue-axios相关的模块文件。

也可以使用图形化工具进行依赖的管理，在项目管理器的依赖管理板块下，可以查看到当前项目安装的依赖有哪些、版本如何，也可以直接在其中进行插件的安装和卸载，如图9-8所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025518.jpeg)

图9-8　使用图形化工具进行依赖管理

可以尝试在图形化工具中进行组件的卸载，单击页面中的“安装依赖”按钮，可以在所有可用的依赖中进行搜索，选择自己需要的进行安装，如图9-9所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030408.jpeg)

图9-9　使用图形化工具安装依赖

另外，vue-axios是一个在Vue中用于网络请求的依赖库，后面的章节会专门介绍它。

### 工程构建

开发完一个Vue项目后，需要将其构建成可发布的代码产品。Vue CLI提供了对应的工具链来实现这些功能。

在Vue工程目录下执行如下命令，可以直接将项目代码编译构建成生产包：

```
     npm run build
```

构建过程可能需要一段时间、构建完成后，在工程的根目录下将会生成一个名为dist的文件夹，这个文件夹就是我们要发布的软件包。可以看到，这个文件夹下包含一个名为index.html的文件，它是项目的入口文件，除此之外，还包含一些静态资源与CSS、JavaScript等相关文件，这些文件中的代码都是被压缩后的。

当然，也可以使用图形化的管理工具来构建工程，如图9-10所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025064.jpeg)

图9-10　使用图形化管理工具构建工程

需要注意，我们不添加任何参数进行构建会按照一些默认的规则进行，例如构建完成后的目标文件将生成在dist文件夹中，默认的构建环境是生产环境（开发环境的依赖不会被添加）。在构建时，也可以对一些构建参数进行配置，以使用图形化工具为例，可配置的参数如图9-11所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030300.jpeg)

图9-11　对构建参数进行配置

### 新一代前端构建工具Vite

现在，你已经了解了Vue CLI的基本使用。Vue CLI是非常优秀的Vue项目构建工具，但它并不是唯一的，如果你追求极致的构建速度，Vite将是一种不错的选择。

#### Vite与Vue CLI的比较

Vue CLI非常适合大型商业项目的开发，它是构建大型Vue项目不可或缺的工具，Vue CLI主要包括工程脚手架、带热重载模块的开发服务器、插件系统、用户界面等功能。与Vue CLI类似，Vite也是一个提供项目脚手架和开发服务器的构建工具。不同的是，Vite不是基于Webpack的，它有一套自己的开发服务器，并且Vite本身并不像Vue CLI那样功能完善且强大，它只专注于提供基本构建的功能和开发服务器。因此，Vite更加小巧迅捷，其开发服务器比基于Webpack的开发服务器快10倍左右，这对开发者来说太重要了，开发服务器的响应速度会直接影响开发者的编程体验和开发效率。对于非常大型的项目来说，可能会有成千上万个JavaScript模块，这时构建效率的速度差异就会非常明显。

虽然Vite在“速度”上无疑比Vue CLI快很多，但其没有用户界面，也没有提供插件管理系统，对于初学者来说并不友好。在实际项目开发中，到底是要使用Vue CLI还是使用Vite并没有一定的标准，读者可以按需选择。



### 小结与练习

在日常开发中，大多数Vue项目都会采用Vue CLI工具来进行创建、开发、打包发布。其流程化的工具链可以大大减少开发者项目搭建和管理的负担。

**练习：**请思考一下Vue CLI是怎样的一个开发工具，如何使用。

**温馨提示：**Vue CLI是一个基于Vue.js进行快速开发的完整系统，其提供了一套可交互式的项目脚手架，无论是项目开发过程中的环境配置、插件和依赖管理，还是工程的构建打包与部署，使用Vue CLI工具都可以极大地简化开发者需要做的工作。Vue CLI也提供了一套完全图形化的管理工具，开发者使用起来更加方便直观。另外，其还配套了一个vue-cli-service服务，可以帮助开发者方便地在开发环境中运行工程。



## Vite



#### 体验Vite构建工具

在创建基于Vite脚手架的Vue项目前，首先要确保我们所使用的Node.js版本大于12.0.0。在终端执行如下指令可以查看当前使用的Node.js的版本：

```
     node -v
```

如果终端输出的Node.js版本号并不大于12.0.0，则有两种处理方式，一种是直接从Node.js的官网下载新的Node.js软件，安装新版本即可，官网地址如下：

```
     http://nodejs.cn/
```

另一种方式是使用NVM来管理Node.js的版本，NVM可以在安装的多个版本的Node.js间任意选择需要使用的，非常方便。关于NVM和Node.js的安装不是本节的重点，这里不再赘述。

确认当前使用的Node.js版本符合要求后，在终端执行如下指令来创建Vue项目工程：

```
     npm init @vitejs/app
```

之后需要一步一步渐进式地来选择一些配置项。首先需要输入工程名和包名，例如可以取名为viteDemo，之后选择要使用的框架，Vite不止支持构建Vue项目，也支持构建基于React等框架的项目，这里选择Vue即可。

项目创建完成后，可以看到生成的工程目录结构如图9-12所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025992.jpeg)

图9-12　Vite创建的Vue项目工程

从目录结构上看，Vite创建的工程与Vue CLI创建的工程十分类似，其中差别在于package.json文件，Vite工程中的代码如下：

```
     {
     "name": "vitedemo",
     "version": "0.0.0",
     "scripts": {
     "dev": "vite",
     "build": "vite build",
     "serve": "vite preview"
     },
     "dependencies": {
     "vue": "^3.2.6"
     },
     "devDependencies": {
     "@vitejs/plugin-vue": "^1.6.1",
     "@vue/compiler-sfc": "^3.2.6",
     "vite": "^2.5.4"
     }
     }
```

在工程目录下执行npm run dev（第一次执行前别忘记先执行npm install指令安装依赖）即可开启开发服务器，执行npm run build即可进行打包操作。此模板工程的运行效果如图9-13所示。

无论选择使用Vue CLI构建工具还是Vite构建工具都没有关系，本书后面介绍的内容都只专注于Vue框架本身的使用，使用任何构建工具都可以完成。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025591.jpeg)

图9-13　Vite创建的Vue项目模板工程





## Nuxt.js

 是一个基于 Vue.js 的通用应用框架，它提供了一些强大的功能和约定，帮助开发者快速搭建 Vue.js 应用。Nuxt.js 构建在 Vue.js 的基础之上，提供了更多功能，旨在简化 Vue.js 应用的开发过程。

### 主要特点和功能：

1. **通用应用**：Nuxt.js 支持创建通用应用（Universal Application，也称为同构应用），这意味着你可以在服务器端渲染页面，也可以在客户端运行，提供更好的 SEO 和首屏加载性能。

2. **自动路由配置**：Nuxt.js 基于文件目录结构自动生成路由配置，不需要手动进行路由配置。页面和路由之间的关系可以通过文件夹和文件名来隐式声明。

3. **预渲染和静态站点生成**：除了通用应用，Nuxt.js 还支持静态站点生成，可以生成预渲染的静态 HTML 文件，这样可以更快地加载页面，并且对于 SEO 友好。

4. **自动优化**：Nuxt.js 自动处理应用程序的性能优化，包括打包、压缩、代码分割、懒加载等，让开发者专注于业务逻辑而不必过多考虑优化细节。

5. **中间件**：Nuxt.js 提供中间件的概念，可以在页面渲染之前或之后执行一些逻辑，比如权限验证、页面过渡等。

6. **插件系统**：Nuxt.js 允许开发者通过插件系统来扩展应用程序的功能，包括添加第三方库、工具、以及自定义功能等。

总的来说，Nuxt.js 提供了一种更结构化、更约定化的开发方式，使得 Vue.js 应用的开发更加简洁、高效，并提供了更多的工具和选项来满足各种应用场景的需求。



### 安装

 create-nuxt-app

首先，确保你已经在本地安装了 Node.js 和 npm。然后打开终端并运行以下命令来全局安装 create-nuxt-app：

```bash
npm install -g create-nuxt-app
```

2. 创建项目名为 my-nuxt-demo

使用 create-nuxt-app 命令创建一个名为 `my-nuxt-demo` 的新项目：

```bash
npx create-nuxt-app my-nuxt-demo
```

这将引导你完成一个设置项目的过程，包括选择项目配置、UI 框架、静态文件托管等选项。根据你的需求进行选择即可。

3. 调试运行

进入到 `my-nuxt-demo` 目录，并启动项目：

```bash
cd my-nuxt-demo
npm run dev
```

这将启动 Nuxt.js 应用程序，并且在本地服务器上运行。你可以在终端看到项目的访问地址。

4. 在浏览器访问

打开浏览器，输入 Nuxt.js 应用程序的访问地址（通常是 `http://localhost:3000`），检查应用程序是否正常运行。

完成以上步骤后，你就创建了一个基于 Nuxt.js 的项目，并可以在其中开始开发登录和注册切换的功能。你可以在 Nuxt.js 中使用页面和组件来构建登录和注册页面，并使用 Vuex 来管理用户登录状态和数据。

# vue-axios--网络框架



#### 使用vue-axios进行数据请求

axios本身是一个基于promise的HTTP客户端工具，vue-axios针对Vue对axios进行了一层简单的包装。在Vue应用中，使用其进行网络数据的请求非常简单。

首先，在Vue项目工程下执行如下指令进行vue-axios模块的安装：

```
     npm install --save axios vue-axios
```

安装完成后，可以检查package.json文件中是否已经添加了vue-axios的依赖。
main.js文件中对其进行导入和注册，代码如下：

```vue
     // 导入vue-axios模块
     import VueAxios from 'vue-axios'
     import axios from 'axios';
     // 导入我们自定义的根组件
     import App from './App.vue'
     // 挂载根组件
     const app = createApp(App)
     // 注册axios
     app.use(VueAxios, axios)
     app.mount('#app')
```

需要注意，在导入我们自定义的组件之前进行vue-axios模块的导入。之后，可以任意一个组件为例，在其生命周期方法中编写如下代码进行请求的测试：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030438.jpeg)

运行代码，打开浏览器的控制台，你会发现请求并没有按照我们的预期方式成功完成，控制台会输出如下信息：

```
    Access to XMLHttpRequest at
'http://apis.juhe.cn/simpleWeather/query?city=%E8%8B%8F%E5%B7%9E&key=cffe158ca
f3fe63aa2959767a503bxxx' from origin 'http://192.168.34.13:8080' has been blocked
by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested
resource.
```

出现此问题的原因是产生了跨域请求，我们在Vue CLI创建的项目中更改全局配置可以解决此问题。首先在Vue项目的根目录下创建vue.config.js文件，在其中编写如下配置项：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025815.jpeg)

修改请求数据的测试代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025980.jpeg)

如以上代码所示，将请求的API接口前的地址强制替换成了字符串“/myApi”，这样请求就能进入我们配置的代理逻辑中。实现跨域请求还有一点需要注意，我们要请求的城市是上海，真正发起请求时，需要将城市进行URI编码。重新运行Vue项目，在浏览器控制台可以看到，已经能够正常地访问接口服务了，如图11-5所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112030594.jpeg)

图11-5　请求到了天气预报数据

通过实例代码可以看到，使用vue-axios进行数据的请求非常简单，在组件内部直接使用this.axios.get方法即可发起GET请求。当然也可以使用this.axios.post方法来发起POST请求，此方法会返回Promise对象，后续可以获取请求成功后的数据或失败的原因。下一节将介绍更多vue-axios中提供的功能接口。

### 实用功能介绍



#### 通过配置的方式进行数据请求

如果要直接进行GET请求，使用如下方法即可：

```
     axios.get(url[, config])
```

其中，url参数是要请求的接口；config参数是选填的，用来配置请求的额外选项。与此方法类似，vue-axios中还提供了下面的常用快捷方法：

```
     // 快捷发起POST请求，data设置请求的参数
     axios.post(url[, data[, config]])
     // 快捷发起DELETE请求
     axios.delete(url[, config])
     // 快捷发起HEAD请求
     axios.head(url[, config])
     // 快捷发起OPTIONS请求
     axios.options(url[, config])
     // 跨界发起PUT请求
     axios.put(url[, data[, config]])
     // 快捷发起PATCH请求
     axios.patch(url[, data[, config]])
```

除了使用这些快捷方法外，也可以完全通过自己的配置来进行数据请求，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025792.jpeg)

通过这种配置方式进行的数据请求效果与使用快捷方法一致。需要注意，在配置时必须设置请求的method方法。

大多数时候，在同一个项目中，使用的请求很多配置都是相同的。对于这种情况，可以创建一个新的axios请求实例，之后所有的请求都使用这个实例来发起，实例本身的配置会与快捷方法的配置合并，这样既能够复用大多数相似的配置，又可以实现某些请求的定制化，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025282.jpeg)

如果需要让某些配置作用于所有请求，即需要重设axios的默认配置，可以使用axios的defaults属性进行配置，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031357.jpeg)

在对请求配置进行合并时，会按照一定的优先级进行选择，优先级排序如下：

```
     axios默认配置 < defaults属性配置 < 请求时的config参数配置
```



#### 请求的配置与响应数据结构

在axios中，无论我们使用配置的方式进行数据请求还是使用快捷方法进行数据请求，都可以传一个配置对象来对请求进行配置，此配置对象可配置的参数非常丰富，如表11-1所示。

表11-1　配置对象可配置的参数

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025048.jpeg)

（续表）

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025083.jpeg)

通过表11-1列出的配置属性，基本可以满足各种场景下的数据请求需求。当一个请求被发出后，axios会返回一个Promise对象，通过此Promise对象可以异步地等待数据的返回，axios返回的数据是一个包装好的对象，其中包装的属性列举如表11-2所示。

表11-2　包装对象的属性

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031256.jpeg)

你可以尝试在浏览器中打印这些数据，观察这些数据中的信息。

#### 拦截器的使用

拦截器的功能在于其允许开发者在请求发起前或请求完成后进行拦截，从而在这些时候添加一些定制化的逻辑。举一个很简单的例子，在请求发送前，可能需要激活页面的Loading特效，在请求完成后移除Loading特效，同时，如果请求的结果是异常的，可能还需要进行一个弹窗提示，而这些逻辑对于项目中的大部分请求来说都是通用的，这时就可以使用拦截器。

要对请求的开始进行拦截，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025280.jpeg)

运行上面的代码，在请求开始前会有弹窗提示。

也可以对请求完成后进行拦截，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031169.jpeg)

在拦截器中，也可以对响应数据进行修改，将修改后的数据返回给请求调用处使用。

需要注意，请求拦截器的添加是和axios请求实例绑定的，后续此实例发起的请求都会被拦截器拦截，但是可以使用如下方式在不需要拦截器的时候将其移除：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025766.jpeg)



### 11.3　范例：实现一个天气预报应用

本节将结合聚合数据提供天气预报接口服务，尝试开发一款天气预报网页应用。前面如果你观察过天气预报接口返回的数据结构，就会发现其中包含两部分数据，一部分是当前的天气信息数据，另一部分是未来几天的天气数据。在页面设计时，也可以分成两个模块，分别展示当日的天气信息和未来几日的天气信息。

#### 11.3.1　搭建页面框架

创建一个名为Weather.vue的组件文件，编写HTML模板代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031915.jpeg)

如以上代码所示，从布局结构上，页面分为头部和主体部分两部分。头部布局了一个输入框，用来输入要查询天气的城市名称；主体部分又分为上下两部分，上部分展示当前的天气信息，下部分为一个列表，展示未来几天的天气信息。

实现简单的CSS样式代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025213.jpeg)

#### 11.3.2　实现天气预报应用的核心逻辑

天气预报组件的JavaScript逻辑代码非常简单，只需要监听用户输入的城市名，进行接口请求，当接口数据返回后，用其来动态地渲染页面即可，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025236.jpeg)

至此，一个功能完整的实用天气预报应用就开发完成了。可以看到，使用Vue及其生态内的其他UI支持模块、网络支持模块等开发一款应用程序真的非常方便。

运行编写好的组件代码，效果如图11-6所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025892.jpeg)

图11-6　天气预报应用页面效果



# 路由管理

是一项关键任务，特别是用于构建单页面应用（SPA）。Vue.js提供了官方的路由库，Vue Router，用于实现客户端端路由，允许您创建多个页面视图并在应用程序中进行导航。以下是有关Vue Router的基本概念和示例：

### 安装和配置Vue Router

首先，您需要安装Vue Router。您可以使用npm或yarn执行以下命令来安装Vue Router：

```bash
npm install vue-router
# 或
yarn add vue-router
```

然后，在您的Vue.js应用程序中，您需要配置Vue Router。通常，您会创建一个路由文件，例如`router.js`，并在其中定义路由配置。

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

const routes = [
  {
    path: '/',
    component: Home
  },
  {
    path: '/about',
    component: About
  },
  // 添加更多路由配置
];

const router = new VueRouter({
  routes
});

export default router;
```

### 路由视图和导航

在Vue Router中，路由是由`path`和`component`定义的，`path`表示URL的路径，`component`表示与该路径关联的Vue组件。您可以在路由配置中定义多个路由，每个路由对应不同的组件。然后，您可以使用`<router-view>`元素来显示当前路由的组件。

```vue
<template>
  <div>
    <router-view></router-view>
  </div>
</template>
```

导航链接通常使用`<router-link>`组件创建，它会自动处理路由的跳转。

```vue
<router-link to="/">Home</router-link>
<router-link to="/about">About</router-link>
```

### 路由参数

您可以在路由中包含参数，以便根据不同的参数值显示不同的内容。参数可以在路由配置中使用冒号定义，并且可以在组件中通过`$route`对象访问。

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User
  }
];
```

```vue
<template>
  <div>
    <p>User ID: {{ $route.params.id }}</p>
  </div>
</template>
```

### 编程式导航

除了使用`<router-link>`进行导航之外，您还可以在组件中使用编程式导航来切换路由。通过`$router`对象，您可以实现在JavaScript代码中进行路由跳转。

```javascript
// 在组件方法中进行路由跳转
this.$router.push('/about');

// 或者使用命名路由
this.$router.push({ name: 'about' });
```

这只是Vue Router的一些基本概念。它还提供了其他功能，如嵌套路由、路由重定向、导航守卫等，用于创建复杂的路由配置和处理路由的生命周期。Vue Router是构建SPA应用的重要工具，它允许您将不同的页面组件与URL路径关联，从而创建具有多个视图的单页应用。

Vue路由是用来管理页面切换或跳转的一种方式。Vue十分适合用来创建单页面应用。所谓单页面应用，不是指“只有一个用户页面”的应用，而是从开发角度上讲的一种架构方式，单页面只有一个主应用入口，通过组件的切换来渲染不同的功能页面。当然，对于组件切换，我们可以借助Vue的动态组件功能，但其管理起来非常麻烦，且不易维护。幸运的是，Vue有配套的路由管理方案——Vue Router，使得我们可以更加自然地进行功能页面的管理。

**通过本章，你将学习到：**

·　Vue Router模块的安装与简单使用。

·　动态路由、嵌套路由的用法。

·　路由的传参方法。

·　为路由添加导航守卫。

·　Vue Router的进阶用法。

### 12.1　Vue Router的安装与简单使用

Vue Router是Vue官方的路由管理器，与Vue框架本身深度契合。Vue Router主要包含如下功能：

·　路由支持嵌套。

·　可以模块化地进行路由配置。

·　支持路由参数、查询和通配符。

·　提供了视图过渡效果。

·　能够精准地进行导航控制。

本节就来一起安装Vue Router模块，并对其功能进行简单的体验。

#### 12.1.1　Vue Router的安装

与Vue框架本身一样，Vue Router支持使用CDN的方式引入，也支持使用NPM的方式进行安装。在本章的示例中，我们采用Vue CLI创建的项目来做演示，将采用NPM的方式来安装Vue。如果需要使用CDN的方式引入，地址如下：

```
     https://unpkg.com/vue-router@4
```

使用Vue CLI创建一个示例的Vue项目工程，使用终端在项目根目录下执行如下指令来安装Vue Router模块：

```
     npm install vue-router@4 -s
```

稍等片刻，安装完成后在项目的package.json文件中会自动添加Vue Router的依赖，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025371.jpeg)

需要注意，你安装的Vue Router版本并不一定需要和书中的一样，但要求是4.x.x系列的版本，因为大版本不同的Vue Router模块接口和功能差异较大。

做完上面的工作后，就可以尝试对Vue Router进行简单使用了。

#### 12.1.2　一个简单的Vue Router使用示例

我们一直在讲路由的作用是页面管理，在实际应用中，需要做的其实非常简单：将定义好的Vue组件绑定到指定的路由，然后通过路由指定在何时或何处渲染这个组件。

首先，创建两个简单的示例组件。在工程的components文件夹下新建两个文件，分别命名为Demo1.vue和Demo2.vue，在其中编写如下示例代码：

Demo1.vue：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025104.jpeg)

Demo2.vue：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025036.jpeg)

Demo1和Demo2这两个组件作为示例使用，非常简单。修改App.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025084.jpeg)

如以上代码所示，router-link组件是一个自定义的链接组件，它比常规的a标签要强大很多，其允许在不重新加载页面的情况下更改页面的URL。router-view用来渲染与当前URL对应的组件，我们可以将其放在任何位置，例如带顶部导航栏的应用，其页面主体内容部分就可以放置router-view组件，通过导航栏上按钮的切换来替换内容组件。

修改项目中的main.js文件，在其中进行路由的定义与注册，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025567.jpeg)

运行上面的代码，单击页面中的两个切换按钮，可以看到对应的内容组件也会发生切换，如图12-1所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026229.jpeg)

图12-1　Vue Router体验

### 12.2　带参数的动态路由

我们已经了解到，不同的路由可以匹配到不同的组件，从而实现页面的切换。有些时候，我们需要将同一类型的路由匹配到同一个组件，通过路由的参数来控制组件的渲染。例如对于“用户中心”这类页面组件，不同的用户渲染的信息是不同的，这时就可以通过为路由添加参数来实现。

#### 12.2.1　路由参数匹配

我们先编写一个示例的用户中心组件，此组件非常简单，直接通过解析路由中的参数来显示当前用户的昵称和编号。在工程的components文件夹下新建一个名为User.vue的文件，在其中编写如下代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025003.jpeg)

如以上代码所示，在组件内部可以使用$route属性获取全局的路由对象，路由中定义的参数可以在此对象的params属性中获取到。在main.js中定义路由如下：

import User from './components/User.vue'

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025824.jpeg)

在定义路由的路径path时，使用冒号来标记参数，如以上代码中定义的路由路径，username和id都是路由的参数，以下路径会被自动匹配：

```
     /user/小王/8888
```

其中“小王”会被解析到路由的username属性，“8888”会被解析到路由的id属性。

现在，运行Vue工程，尝试在浏览器中输入如下格式的地址：

```
     http://localhost:8080/#/user/小王/8888
```

你会看到，页面的加载效果如图12-2所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025437.jpeg)

图12-2　解析路由中的参数

需要注意，在使用带参数的路由时，对应相同组件的路由在进行导航切换时，相同的组件并不会被销毁再创建，这种复用机制使得页面的加载效率更高。但这也表明，页面切换时，组件的生命周期方法都不会被再次调用，如果需要通过路由参数来请求数据，之后渲染页面需要特别注意，不能在生命周期方法中进行数据请求逻辑。例如，修改App.vue组件的模板代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026607.jpeg)

修改User.vue的代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025035.jpeg)

我们模拟在组件挂载时根据路由参数来进行数据的请求。运行代码可以看到，单击页面上的链接进行组件切换时，User组件中显示的用户名称和用户编号都会实时地刷新，但是alert弹窗只有在User组件第一次加载时才会弹出，后续不会再弹出。对于这种场景，我们可以采用导航守卫的方式来处理，每次路由参数有更新，都会回调守卫函数。修改User.vue组件中的JavaScript代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025670.jpeg)

再次运行代码，当同一个路由的参数发生变化时，也会有alert弹出提示。beforeRouteUpdate函数在路由将要更新时会调用，其会传入两个参数，to是更新后的路由对象，from是更新前的路由对象。

#### 12.2.2　路由匹配的语法规则

在进行路由参数匹配时，Vue Router允许参数内部使用正则表达式来进行匹配。首先来看一个例子。在上一节中，我们提供了User组件做路由示范，将其修改如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026999.jpeg)

同时，在components文件夹下新建一个名为UserSetting.vue的文件，在其中编写如下代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025845.jpeg)

我们将User组件作为用户中心页面来使用，而UserSetting组件作为用户设置页面来使用。这两个页面所需要的参数不同，用户中心页面需要用户名参数，用户设置页面需要用户编号参数。我们可以在main.js文件中定义路由：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025321.jpeg)

你会发现，上面的代码中定义的两个路由除了参数名不同外，格式完全一样。这种情况下，我们无法访问用户设置页面，所有符合UserSetting组件的路由规则同时也符合User组件。为了解决这一问题，最简单的方式是加一个静态的前缀路径，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025905.jpeg)

这是一个好方法，但并不是唯一的方法。对于本示例来说，用户中心页面和用户设置页面所需要参数的类型有明显的差异，用户编号必须是数值，用户名则不能是纯数字。因此，可以通过正则约束来实现不同类型的参数匹配到对应的路由组件，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025872.jpeg)

这样，对于“/user/6666”这样的路由就会匹配到UserSetting组件，“/user/小王”这样的路由就会匹配到User组件。

在正则表达式中，“*”可以用来匹配0个或多个，“+”可以用来匹配1个或多个。在路由定义时使用这两个符号可以实现多级参数。在components文件夹下新建一个名为Category.vue的示例组件，编写如下代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025914.jpeg)

在main.js中增加如下路由定义：

```
     { path: '/category/:cat*', component:Category}
```

需要注意，别忘了在main.js文件中对Category组件进行引入。当我们采用多级匹配的方式来定义路由时，路由中传递的参数会自动转换成一个数组，例如路由“/category/一级/二级/三级”可以匹配到上面定义的路由，匹配成功后，cat参数为一个数组，其中数据为["一级","二级","三级"]。

有时候，页面组件所需要的参数并不都是必传的，以用户中心页面为例，当传了用户名参数时，其需要渲染登录后的用户中心状态，当没有传用户名参数时，其可能需要渲染未登录时的状态。这时，可以将此username参数定义为可选的，示例如下：

```
     { path: '/user/:username?', component:User }
```

参数被定义为可选后，路由中不包含此参数的时候也可以正常匹配到指定的组件。

#### 12.2.3　路由的嵌套

前面定义了很多路由，但是真正渲染路由的地方只有一个，即只有一个<router-view></router-view>出口，这类路由实际上都是顶级路由。在实际开发中，我们的项目可能非常复杂，除了根组件中需要路由外，一些子组件中可能也需要路由，Vue Router提供了嵌套路由技术来支持这类场景。

以之前创建的User组件为例，假设组件中有一部分用来渲染用户的好友列表，这部分也可以用组件来完成。首先在components文件夹下新建一个名为Friends.vue的文件，编写代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025980.jpeg)

Friends组件只会在用户中心使用，可以将其作为一个子路由进行定义。首先修改User.vue代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025955.jpeg)

需要注意，User组件本身也是由路由管理的，在User组件内部再使用的<router-view></router-view>标签实际上定义的是二级路由的页面出口。在main.js中定义二级路由如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031955.jpeg)

需要注意，在定义路由时，不要忘记在main.js中引入此组件。之前在定义路由时都只使用了path和component属性，其实每个路由对象本身也可以定义子路由对象。理论上讲，可以根据自己的需要来定义路由嵌套的层数，通过路由的嵌套，可以更好地对路由进行分层管理。如以上代码所示，当我们访问如下路径时，页面效果如图12-3所示。

```
     /user/小王/friends/6
```

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025778.jpeg)

图12-3　路由嵌套示例

### 12.3　页面导航

导航本身是指页面间的跳转和切换。router-link组件就是一种导航组件。我们可以设置其属性to来指定要执行的路由。除了使用route-link组件外，还有其他的方式进行路由控制，任意可以添加交互方法的组件都可以实现路由管理。本节将介绍通过函数的方式进行路由跳转。

#### 12.3.1　使用路由方法

当我们成功向Vue应用注册路由后，在任何Vue实例中，都可以通过$route属性访问路由对象。
通过调用路由对象的push方法可以向history栈中添加一个新的记录。也就是说，用户可以通过浏览器的返回按钮返回上一个路由的URL。

首先，修改App.vue文件代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025474.jpeg)

如以上代码所示，我们使用按钮组件代替之前的router-link组件，在按钮的单击方法中进行路由的跳转操作。push方法可以接收一个对象，对象中通过path属性配置其URL路径。push方法也支持直接传入一个字符串作为URL路径，代码如下：

```
     this.$router.push("/user/小王")
```



也可以通过路由名加参数的方式让Vue Router自动生成URL，要使用这种方法进行路由跳转，在定义路由的时候需要对路由进行命名，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025002.jpeg)

之后，可以使用如下方式进行路由跳转：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031285.jpeg)

如果路由需要查询参数，可以通过query属性进行设置，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025968.jpeg)





需要注意，在调用push方法配置路由对象时，如果设置了path属性，则params属性会被自动忽略。

push方法本身也会返回一个Promise对象，可以用其来处理路由跳转成功之后的逻辑，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025220.jpeg)

#### 12.3.2　导航历史控制

当我们使用router-link组件或push方法切换页面时，新的路由实际上会被放入history导航栈中，用户可以灵活地使用浏览器的前进和后退功能在导航栈路由中进行切换。对于有些场景，我们不希望导航栈中的路由增加，这时可以配置replace参数或直接调用replace方法来进行路由跳转，这种方式跳转的页面会直接替换当前的页面，即跳转前页面的路由从导航栈中删除。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025861.jpeg)

Vue Router提供了另一个方法，让我们可以灵活地选择跳转到导航栈中的某个位置，示例如下：

```
     // 跳转到后1个记录
     this.$router.go(1)
     // 跳转到后3个记录
     this.$router.go(3)
     // 跳转到前1个记录
     this.$router.go(-1)
```





### 12.4　关于路由的命名

我们知道，在定义路由时，除了path之外，还可以设置name属性，name属性为路由提供了名称，使用名称进行路由切换比直接使用path进行切换有很明显的优势：避免硬编码URL、可以自动处理参数的编码等。

#### 12.4.1　使用名称进行路由切换

与使用path路径进行路由切换类似，router-link组件和push方法都可以根据名称进行路由切换。以前面编写的代码为例，定义用户中心的名称为user，使用如下方法可以直接进行切换：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025962.jpeg)

使用router-link组件切换的示例如下：

```
    <router-link :to="{ name: 'user', params: { username: '小王' }}">小王
</router-link>
```



#### 12.4.2　路由视图命名

路由视图命名是指对router-view组件进行命名，router-view组件用来定义路由组件的出口，前面我们讲过，路由支持嵌套，router-view可以进行嵌套。通过嵌套，允许我们的Vue应用中出现多个router-view组件。但是对于有些场景，可能需要同级地展示多个路由视图，例如顶部导航区和主内容区两部分都需要使用路由组件，这时候就需要同级地使用router-view组件，要定义同级的每个router-view要展示的组件，可以对其进行命名。

修改App.vue文件，将页面的布局分为头部和内容主体两部分，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025224.jpeg)

在mian.js文件中定义一个新的路由，设置如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031994.jpeg)

之前定义路由时，一个路径只对应一个组件，其实也可以通过components来设置一组组件，components需要设为一个对象，其中的键表示页面中路由视图的名称，值为要渲染的组件。在上面的例子中，页面的头部会被渲染为User组件，主体部分会被渲染为UserSetting组件，如图12-4所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031467.jpeg)

图12-4　进行路由视图的命名

需要注意，对于没有命名的router-view组件，其名字会被默认分配为default，如果编写组件模板如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025932.jpeg)

使用如下方式定义路由效果是一样的：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025477.jpeg)

温馨提示：在嵌套的子路由中，也可以使用视图命名路由，对于结构复杂的页面，可以先将其按照模块进行拆分，梳理清晰路由的组织关系再进行开发。



#### 12.4.3　使用别名

别名提供了一种路由路径映射的方式，也就是说我们可以自由地将组件映射到一个任意的路径上，而不用受到嵌套结构的限制。

首先，可以尝试为一个简单的一级路由来设置别名，修改用户设置页面的路由定义如下：

```
     const routes = [
       { path: '/user/:id(\\d+)', component:UserSetting, alias: '/setting/:id' }
     ]
```

之后，下面两个路径的页面渲染效果将完全一样：

```
     http://localhost:8080/#/setting/6666/
     http://localhost:8080/#/user/6666/
```

需要注意，别名和重定向并不完全一样，别名不会改变用户在浏览器中输入的路径本身，对于多级嵌套的路由来说，可以使用别名在路径上对其进行简化。如果原路由有参数配置，一定要注意别名也需要对应地包含这些参数。在为路由配置别名时，alias属性可以直接设置为别名字符串，也可以设置为数组同时配置一组别名，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031167.jpeg)

#### 12.4.4　路由重定向

重定向也是通过路由配置来完成的，与别名的区别在于，重定向会将当前路由映射到另一个路由上，页面的URL会产生改变。例如当用户访问路由'/d/1'时，需要页面渲染'/demo1'路由对应的组件，配置方式如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031981.jpeg)

redirect也支持配置为对象，设置对象的name属性可以直接指定命名路由，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025096.jpeg)

上面的示例代码都是采用静态的方式配置路由重定向的，在实际开发中，更多时候会采用动态的方式配置重定向，例如对于需要用户登录才能访问的页面，当未登录的用户访问此路由时，我们自动将其重定向到登录页面，下面的示例代码将模拟这一过程：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025504.jpeg)



### 12.5　关于路由传参

通过前面的学习，我们对Vue Router的基本使用已经有了初步的了解，在进行路由跳转时，可以通过参数的传递来进行后续的逻辑处理。在组件内部，之前使用$route.params的方式来获取路由传递的参数，这种方式虽然可行，但组件与路由紧紧地耦合在了一起，并不利于组件的复用性。本节来讨论一下路由的另一种传参方式——使用属性的方式进行参数传递。

还记得我们编写的用户设置页面是如何获取路由传递的id参数的吗？代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025598.jpeg)

由于之前在组件的模板内部使用了$route属性，这导致此组件的通用性大大降低，首先将其所有耦合路由的地方去除掉，修改如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025917.jpeg)

现在，UserSetting组件能够通过外部传递的属性来做内部逻辑，后面需要做的只是将路由的传参映射到外部属性上。Vue Router默认支持这一功能。路由配置方式如下：

```
     const routes = [
       { path: '/user/:id(\\d+)', component:UserSetting, props:true }
     ]
```

在定义路由时，将props设置为true，则路由中传递的参数会自动映射到组件定义的外部属性，使用十分方便。

对于有多个页面出口的同级命名视图，我们需要对每个视图的props单独进行设置，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025642.jpeg)



如果组件内部需要的参数与路由本身并没有直接关系，也可以将props设置为对象，此时props设置的数据将原样传递给组件的外部属性，例如：

```
     const routes = [
       { path: '/user/:id(\\d+)', component:UserSetting, props:{id:'000'} },
     ]
```

如以上代码所示，此时路由中的参数将被弃用，组件中获取到的id属性值将固定为“000”。

props还有一种更强大的使用方式，可以直接将其设置为一个函数，函数中返回要传递到组件的外部属性对象，这种方式动态性很好，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025169.jpeg)



### 12.6　路由导航守卫

关于导航守卫，前面也使用过，顾名思义，导航守卫的主要作用是在进行路由跳转时决定通过此次跳转或拒绝此次跳转。在Vue Router中有多种方式来定义导航守卫。

#### 12.6.1　定义全局的导航守卫

在main.js文件中，我们使用createRouter方法来创建路由实例，此路由实例可以使用beforeEach方法来注册全局的前置导航守卫，之后当触发导航跳转时，都会被此导航守卫捕获，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025794.jpeg)

当注册的beforeEach方法返回的是布尔值时，其用来决定是否允许此次导航跳转，如以上代码所示，所有的路由跳转都将被禁止。

更多时候，我们会在beforeEach方法中返回一个路由配置对象来决定要跳转的页面，这种方式更加灵活，例如可以将登录态校验的逻辑放在全局的前置守卫中处理，非常方便。示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025579.jpeg)

与定义全局前置守卫类似，也可以注册全局的导航后置回调。与前置守卫不同的是，后置回调不会改变导航本身，但是其对页面的分析和监控十分有用。示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025681.jpeg)

路由实例的afterEach方法中设置的回调函数除了接收to和from参数外，还会接收一个failure参数，通过它开发者可以对导航的异常信息进行记录。

#### 12.6.2　为特定的路由注册导航守卫

如果只有特定的场景需要在页面跳转过程中实现相关逻辑，也可以为指定的路由注册导航守卫。有两种注册方式，一种是在配置路由时进行定义，另一种是在组件中进行定义。

在对导航进行配置时，可以直接为其设置beforeEnter属性，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031255.jpeg)

如以上代码所示，当用户访问“/demo1”路由对应的组件时都会被拒绝掉。需要注意，beforeEnter设置的守卫只有在进入路由时会触发，路由的参数变化并不会触发此守卫。

在编写组件时，也可以实现一些方法来为组件定制守卫函数，示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031699.jpeg)

如以上代码所示，beforeRouteEnter是组件的导航前置守卫，在通过路由将要切换到当前组件时被调用，在这个函数中，可以做拦截操作，也可以做重定向操作，需要注意此方法只有在第一次切换此组件时会被调用，路由参数的变化不会重复调用此方法。beforeRouteUpdate方法在当前路由发生变化时会被调用，例如路由参数的变化等都可以在此方法中捕获到。beforeRouteLeave方法会在将要离开当前页面时被调用。还有一点需要特别注意，在beforeRouteEnter方法中不能使用this来获取当前组件实例，因为在导航守卫确认通过前，新的组件还没有被创建。如果一定要在导航被确认时使用当前组件实例处理一些逻辑，可以通过next参数注册回调方法，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025668.jpeg)

当前置守卫确认了此次跳转后，next参数注册的回调方法会被执行，并且会将当前组件的实例作为参数传入。在beforeRouteUpdate和beforeRouteLeave方法中可以直接使用this关键字来获取当前组件实例，无须额外的操作。

下面来总结Vue Router导航跳转的全过程。

第1步：导航被触发，可以通过router-link组件触发，也可以通过$router.push或直接改变URL触发。

第2步：在将要失活的组件中调用beforeRouteLeave守卫函数。

第3步：调用全局注册的beforeEach守卫。

第4步：如果当前使用的组件没有变化，调用组件内的beforeRouteUpdate守卫。

第5步：调用在定义路由时配置的beforeEnter守卫函数。

第6步：解析异步路由组件。

第7步：在被激活的组件中调用beforeRouteEnter守卫。

第8步：导航被确认。

第9步：调用全局注册的afterEach守卫。

第10步：触发DOM更新，页面进行更新。

第11步：调用组件的beforeRouteEnter函数汇总next参数注册的回调函数。



### 12.7　动态路由

截至目前，我们所有使用到的路由都是采用静态配置的方式定义的，即先在main.js中完成路由的配置，再在项目中使用。但某些情况下，可能需要在运行的过程中动态添加或删除路由，Vue Router中也提供了方法支持动态地对路由进行操作。

#### 12.7.1　动态添加与删除路由

在Vue Router中，动态操作路由的方法主要有两个：addRoute和removeRoute，addRoute用来动态添加一条路由，对应的removeRoute用来动态删除一条路由。首先，修改Demo1，Vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025850.jpeg)

我们在Demo1组件中布局了一个按钮元素，在Demo1组件创建完成后，使用addRoute方法动态添加了一条路由，当单击页面上的按钮时，切换到Demo2组件。修改main.js文件中配置路由的部分如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031802.jpeg)

可以尝试一下，如果直接在浏览器中访问“/demo2”页面会报错，因为此时注册的路由列表中并没有此项路由记录，但是如果先访问“/demo1”页面，再单击页面上的按钮进行路由跳转，则能够正常跳转。

在下面几种场景下会触发路由的删除。

当使用addRoute方法动态添加路由时，如果添加了重名的路由，旧的会被删除，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025595.jpeg)

上面的代码中，路径为“/demo”的路由将会被删除。

在调用addRoute方法时，它其实会返回一个删除回调，我们也可以通过此删除回调来直接删除所添加的路由，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025300.jpeg)

另外，对于已经命名的路由，也可以通过名称来对路由进行删除，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025021.jpeg)

需要注意，当路由被删除时，其所有的别名和子路由也会同步被删除。在Vue Router中，还提供了方法来获取现有的路由，例如：

```
     console.log(this.$router.hasRoute("Demo2"));
     console.log(this.$router.getRoutes());
```

其中，hasRouter方法用来检查当前已经注册的路由中是否包含某个路由，getRoutes方法用来获取包含所有路由的列表。

### 12.8　小结与练习

本章介绍了Vue Router模块的使用方法，路由技术在实际项目开发中应用广泛，随着网页应用的功能越来越强大，前端代码也将越来越复杂，因此如何高效清晰地根据业务模块组织代码变得十分重要。路由就是一种非常优秀的页面组织方式，通过路由可以将页面按照组件的方式进行拆分，组件内只关注内部的业务逻辑，组件间通过路由来进行交互和跳转。

通过本章的学习，相信你已经有了开发大型前端应用的基本能力，可以尝试模仿流行的互联网应用，通过路由来搭建一些页面进行练习。

**练习：**在实际的应用开发中，只有一个页面的应用很少，页面间的组织与管理非常重要。那么你能简述路由管理在前端开发中解决了什么问题吗？

**温习提示**：可以从解耦、模块化等方面进行分析。

# 状态管理

是一种在前端应用程序中有效管理和共享数据的方法，特别适用于大型或复杂的应用。它允许您将数据从一个组件传递到另一个组件，以便它们可以协同工作，而不需要通过组件树手动传递数据。状态管理通常用于单页面应用（SPA）中，以管理应用的全局状态、数据和状态变化。

在Vue.js、React和其他现代前端框架中，状态管理通常基于以下核心概念：

1. **全局状态**：状态管理允许您在整个应用程序中维护一个全局状态。这个状态包括应用程序的数据、配置和其他共享信息。

2. **状态存储**：状态通常存储在一个全局状态存储对象中，该对象可以在整个应用程序中访问。在Vue.js中，这个对象通常叫做"store"，而在React中，您可以使用库（如Redux）来创建存储。

3. **状态更改**：组件可以通过触发"操作"或"行为"来请求对状态的更改。这些操作将提交给状态管理，然后状态管理负责更新状态。

4. **状态的响应性**：当状态发生更改时，与之相关的组件会自动更新以反映新的状态。这是通过框架的响应性机制实现的。

以下是关于状态管理的更具体信息：

**Vue.js中的状态管理**：Vue.js的官方状态管理工具是Vuex。Vuex提供了状态存储、操作和计算属性的机制，允许您在Vue应用中管理全局状态。Vuex的核心概念包括`state`（状态）、`mutations`（状态更改），`actions`（操作）和`getters`（计算属性）。

```javascript
// 一个简单的Vuex示例
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    },
    decrement(state) {
      state.count--;
    }
  }
});

export default store;
```

**React中的状态管理**：在React中，状态管理通常使用第三方库来实现，如Redux或Mobx。这些库提供了全局状态存储、操作和连接React组件的机制。

```javascript
// 一个简单的Redux示例
import { createStore } from 'redux';

// Reducer函数
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};

// 创建Redux store
const store = createStore(counterReducer);

export default store;
```

状态管理是一种有助于管理应用程序中复杂状态和数据的有效方式，特别是对于大型和复杂的应用程序。它提供了一种途径，通过它组件可以访问和修改全局状态，而无需手动传递数据。不同的前端框架提供了不同的状态管理工具，开发者可以根据项目需求选择合适的工具来实现状态管理。





首先，Vue框架本身就有状态管理的能力，我们在开发Vue应用页面时，视图上渲染的数据就是通过状态来驱动的。本章主要讨论基于Vue的状态管理框架Vuex，Vuex是一个专为Vue定制的状态管理模块，其集中式地存储和管理应用的所有组件的状态，使这些状态数据可以按照我们预期的方式变化。

当然，并非所有Vue应用的开发都需要使用Vuex来进行状态管理，对于小型的、简单的Vue应用，我们使用Vue自身的状态管理功能就已经足够，但是对于复杂度高、组件繁多的Vue应用，组件间的交互会使得状态的管理变得困难，这时就需要Vuex的帮助了。

**通过本章，你将学习到：**

·　Vuex框架的安装与简单使用。

·　多组件共享状态的管理方法。

·　多组件驱动同一状态的方法。

### 13.1　认识Vuex框架

Vuex采用集中的方式管理所有组件的状态，相较于“集中式”而言，Vue本身对状态的管理是“独立式”的，即每个组件只负责维护自身的状态。

#### 13.1.1　关于状态管理

我们先从一个简单的示例组件来理解状态管理。使用Vue CLI工具新建一个Vue项目工程，为了方便测试，我们将默认生成的部分代码先清理掉，修改App.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025045.jpeg)

修改HelloWorld.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031469.jpeg)

上面的代码逻辑非常简单，页面上渲染了一个按钮组件和一个文本标题，当用户单击按钮时，标题上显示的计数会自增。分析上面的代码可以发现，在Vue应用中，组件状态的管理由如下几部分组成：

##### 1．状态数据

状态数据是指data函数中返回的数据，这些数据自带响应性，由其来对视图的展现进行驱动。

##### 2．视图

视图是指template里面定义的视图模板，其通过声明的方式将状态映射到视图上。

##### 3．动作

动作是指会引起状态变化的行为，即上面代码methods选项中定义的方法，这些方法用来改变状态数据，状态数据的改动最终驱动视图的刷新。

上面3部分的协同工作就是Vue状态管理的核心。总体来看，在这个状态管理模式中，数据的流向是单向的、私有的。由视图触发动作，由动作改变状态，由状态驱动视图。此过程如图13-1所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025862.jpeg)

图13-1　单向数据流使用图

单向数据流这种状态管理模式非常简洁，对于组件不多的简单Vue应用来说，这种模式非常高效，但是对于多组件复杂交互的场景，使用这种方式进行状态管理就会比较困难。我们来思考下面两种状况：

（1）有多个组件依赖于同一个状态。

（2）多个组件都可能触发动作变更一个状态。

对于状况（1），使用上面所述的状态管理方法很难实现，对于嵌套的多个组件，我们还可以通过传值的方式来传递状态，但是对于平级的多个组件，共享同一个状态是非常困难的。

对于状况（2），不同的组件若要更改同一个状态，最直接的方式是将触发动作交给上层，对于多层嵌套的组件，则需要一层一层地向上传递事件，在最上层统一处理状态的更改，这会使代码的维护难度大大增加。

Vuex就是基于这种应用场景产生的，在Vuex中，可以将需要组件间共享的状态抽取出来，以一个全局的单例模式进行管理。在这种模式下，视图无论在视图树中的哪个位置，都可以直接获取这些共享的状态，也可以直接触发修改动作来动态改变这些共享的状态。

#### 13.1.2　安装与体验Vuex

与前面我们使用过的模块的安装方式类似，使用npm命令可以非常方便地为工程安装Vuex模块，命令如下：

```
     npm install vuex@next --save
```

在安装过程中，如果有权限相关的错误产生，可以在命令前添加sudo。安装完成后，即可在工程的package.json文件中看到相关的依赖配置以及所安装的Vuex的版本，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025878.jpeg)

下面尝试体验Vuex状态管理的基本功能。首先仿照HelloWorld组件来创建一个新的组件，命名为HelloWorld2.vue，其功能也是一个简单的计数器，代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025075.jpeg)

修改App.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025825.jpeg)

运行此Vue工程，在页面上可以看到两个计数器，如图13-2所示。

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031167.jpeg)

图13-2　示例工程运行效果

此时，这两个计数器组件是相互独立的，即单击第1个按钮只会增加第1个计数器的值，单击第2个按钮只会增加第2个计数器的值。如果需要让这两个计数器共享一个状态，且同时操作此状态，则需要Vuex出马。

Vuex框架的核心是store，即仓库。简单理解，store本身就是一个容器，其内存储和管理的应用中需要多组件共享的状态。Vuex中的store非常强大，其中存储的状态是响应式的，若store中的状态数据发生变化，其会自动反映到对应的组件视图上。并且，store中的状态数据并不允许开发者直接进行修改，改变store中状态数据的唯一办法是提交mutation操作，通过这样严格的管理，可以更加方便地追踪每一个状态的变化过程，帮助我们进行应用的调试。

现在，我们来使用Vuex对上面的代码进行改写，在main.js文件中编写如下代码：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025816.jpeg)

之后就可以在组件中共享count状态了，并且通过提交increment操作来修改此状态。修改HelloWorld与HelloWorld2组件代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112031584.jpeg)

可以看到，在组件中使用$store属性可以直接获取到store实例，此实例的state属性中存储着所有共享的状态数据，且是响应式的，可以直接绑定到组件的视图进行使用。当需要对状态进行修改时，需要调用store实例的commit方法来提交变更操作，在这个方法中直接传入要执行更改操作的方法名即可。

再次运行工程，你会发现页面上的两个计数器的状态已经能够联动起来。后面，我们将讨论更多Vuex的核心概念。

### 13.2　Vuex中的一些核心概念

本节主要讨论Vuex中的5个核心概念：state、getter、mutation、action和module。

#### 13.2.1　Vuex中的状态state

我们知道，状态实际上就是应用中组件需要共享的数据。在Vuex中采用单一状态树来存储状态数据，也就是说我们的数据源是唯一的。在任何组件中，都可以使用如下方式来获取任何一个状态树中的数据：

```
     this.$store.state
```

当组件中所使用的状态数据非常多时，这种写法就会显得比较烦琐，我们也可以使用Vuex中提供的mapState方法来将其直接映射成组件的计算属性进行使用。由于状态数据本身具有响应性，因此将其映射为计算属性后也具有响应性，使用计算属性和直接使用状态数据并无不同。示例代码如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025435.jpeg)

如果组件使用的计算属性的名字与store中定义的状态名字不一致，也可以在mapState中传入对象来进行配置，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032040.jpeg)

虽然使用Vuex管理状态非常方便，但是这并不意味着需要将组件所有使用到的数据都放在store中，这会使store仓库变得巨大且容易产生冲突。对于那些完全是组件内部使用的数据，还是应该将其定义为局部的状态。

#### 13.2.2　Vuex中的Getter方法

在Vue中，计算属性实际上就是Getter方法，当我们需要将数据处理过再进行使用时，就可以使用计算属性。对于Vuex来说，借助mapState方法，可以方便地将状态映射为计算属性，从而增加一些所需的业务逻辑。但是如果有些计算属性是通用的，或者这些计算属性也是多组件共享的，此时在这些组件中都实现一遍这些计算方法就显得非常多余。Vuex允许我们在定义store实例时添加一些仓库本身的计算属性，即Getter方法。

以上一节编写的示例代码为基础，修改store定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112025310.jpeg)

Getter方法本身也是具有响应性的，当其内部使用的状态发生改变时，其也会触发所绑定组件的更新。在组件中使用store的Getter数据方法如下：

```
     <template>
       <h1>计数器1:{{ this.$store.getters.countText }}</h1>
       <button @click="increment">增加</button>
     </template>
```

Getter方法中也支持参数的传递，这时需要让其返回一个函数，在组件对其进行使用时非常灵活，例如修改countText方法如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032871.jpeg)

通过如下方式对其进行使用即可：

```
     this.$store.getters.countText("次")
```

对于Getter方法，Vuex中也提供了一个方法用来将其映射到组件内部的计算属性中，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032156.jpeg)

#### 13.2.3　Vuex中的Mutation

在Vuex中，修改store中的某个状态数据的唯一方法是提交Mutation。Mutation的定义非常简单，我们只需要将数据变动的行为封装成函数，配置在store实例的mutations选项中即可。在前面编写的示例代码中，我们曾使用Mutation来触发计数器数据的自增，定义的方法如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032283.jpeg)

在需要触发此Mutation时，需要调用store实例的commit方法进行提交，其中使用函数名标明要提交的具体修改指令，例如：

```
     this.$store.commit('increment')
```

在调用commit方法提交修改的时候，也支持传递参数，如此可以使得Mutation方法变得更加灵活。例如上面的例子，可以将自增的大小作为参数，修改Mutation定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026112.jpeg)

提交修改的相关代码示例如下：

```
     this.$store.commit('increment', 2)
```

运行工程，可以发现计数器的自增步长已经变成了2。虽然Mutation方法中参数的类型是任意的，但是我们最好使用对象来作为参数，这样做可以很方便地进行多参数的传递，也支持采用对象的方式进行Mutation方法的提交。例如修改Mutation定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026334.jpeg)

之后，就可以使用如下风格的代码来进行状态修改的提交了：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032017.jpeg)

其中，type表示要调用的修改状态的方法名，其他的属性即要传递的参数。

#### 13.2.4　Vuex中的Action

Action是我们将要接触到的一个新的Vuex中的核心概念。我们知道，要修改store仓库中的状态数据，需要通过提交Mutation来实现。但是Mutation有一个很严重的问题：其定义的方法必须是同步的，即只能同步地对数据进行修改。在实际开发中，并非所有修改数据的场景都是同步的，例如从网络请求获取数据，之后刷新页面。当然，也可以将异步的操作放到组件内部处理，异步操作结束后再提交修改到store仓库，但这样可能会使本来可以复用的代码要在多个组件中分别编写一遍。Vuex中提供了Action来处理这种场景。

Action与Mutation类似，不同的是，Action并不会直接修改状态数据，而是对Mutation进行包装，通过提交Mutation来实现状态的改变。这样在Action定义的方法中，允许我们包含任意的一步操作。

以前面编写的示例代码为基础，修改store实例的定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026041.jpeg)

可以看到，actions中定义的asyncIncrement方法实际上是异步的，其中延迟了3秒才进行状态数据的修改。并且，Action本身是可以接收参数的，其第一个参数是默认的，是与store实例有着相同方法和属性的context上下文对象；第二个参数是自定义参数，由开发者定义，这与Mutation的用法类似。需要注意的是，在组件中使用Action时，需要通过store实例对象的dispatch方法来触发，例如：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026239.jpeg)

对于Action来说，其允许进行异步操作，但是并不是说其必须进行异步操作。在Action中也可以定义同步的方法，只是在这种场景下，其与Mutation的功能完全一样。

#### 13.2.5　Vuex中的Module

Module是Vuex进行模块化编程的一种方式。前面我们分析过，在定义store仓库时，无论是其中的状态，还是Mutation和Action行为，都是共享的，可以将其理解为通过store单例来统一管理。在这种情形下，所有的状态都会集中到同一个对象中，虽然使用起来并没有什么问题，但是过于复杂的对象会使阅读和维护变得困难。为了解决此问题，Vuex中引入了Module模块的概念。

Vuex允许我们将store分割成模块，每个模块拥有自己的state、mutations、actions、getters，甚至可以嵌套拥有自己的子模块。我们先来看一个例子。

首先修改main.js文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032717.jpeg)

如以上代码所示，我们创建了两个模块，两个模块中分别定义了不同的状态和Mutation方法。在使用时，提交Mutation的方式和之前并没有什么不同，在使用状态数据时，则需要区分模块，例如修改HelloWorld.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026361.jpeg)

修改HelloWorld2.vue文件如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026089.jpeg)

此时，两个组件各使用各自模块内部的状态数据，进行状态修改时，使用的也是各自模块内部的Motation方法，这两个组件从逻辑上实现了模块的分离。

Vuex模块化的本意是store进行拆分和隔离。你可能已经发现了，目前虽然模块中的状态数据进行了隔离，但是实际上Mutation依然是共用的，在触发Mutation的时候，也没有进行模块的区分，如果需要更高的封装度与复用性，可以开启模块的命名空间功能，这样模块内部的Getter、Action以及Mutation都会根据模块的嵌套路径进行命名，实际上实现了模块间的完全隔离。例如修改模块定义如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026699.jpeg)

此时这两个模块在命名空间上实现了分离，需要通过如下方式进行使用：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112026996.jpeg)

Vuex中的Module还有一个非常实用的功能，其支持动态注册，这样在编写代码时，我们可以根据实际需要决定是否新增一个Vuex的store模块。要进行模块的动态注册，直接调用store实例的registerModule即可，示例如下：

![img](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407112032196.jpeg)

# vue.config.js



`vue.config.js` 是一个配置文件，用于配置Vue.js应用程序的行为。您可以在项目的根目录中创建此文件，然后在其中指定各种选项，以自定义和配置Vue应用的构建过程、开发服务器、插件等。

以下是一些常见的配置选项，您可以在 `vue.config.js` 文件中使用：

1. **publicPath**：用于指定应用的基本URL路径，通常用于部署到不同的子路径或域名上。

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/my-app/' : '/'
}
```

2. **outputDir**：指定构建产出的目录名称。

```javascript
module.exports = {
  outputDir: 'dist'
}
```

3. **assetsDir**：指定静态资源文件（如CSS、JS、图片等）的输出目录。

```javascript
module.exports = {
  assetsDir: 'static'
}
```

4. **devServer**：用于配置开发服务器选项，包括端口、代理、热模块替换（HMR）等。

```javascript
module.exports = {
  devServer: {
    port: 8080,
    proxy: 'http://localhost:3000'
  }
}
```

5. **chainWebpack**：允许您在Webpack配置中进行更高级的自定义。

```javascript
module.exports = {
  chainWebpack: config => {
    // 在Webpack配置中添加自定义选项
  }
}
```

6. **configureWebpack**：允许您提供一个已经创建的Webpack配置对象或在配置基础上进行更多的自定义。

```javascript
module.exports = {
  configureWebpack: {
    // 自定义Webpack配置
  }
}
```

7. **pluginOptions**：用于配置Vue插件的选项。

```javascript
module.exports = {
  pluginOptions: {
    myPlugin: {
      // 配置选项
    }
  }
}
```

8. **lintOnSave**：用于配置是否在保存文件时进行代码风格检查。

```javascript
module.exports = {
  lintOnSave: true
}
```

9. **transpileDependencies**：用于配置哪些依赖模块需要通过Babel进行转译。

```javascript
module.exports = {
  transpileDependencies: ['my-module']
}
```

`vue.config.js` 文件允许您在Vue.js应用程序的构建和开发过程中进行广泛的自定义。您可以根据项目需求在这个配置文件中添加和修改各种选项，以满足应用程序的特定需求。请注意，配置选项的名称和语法会根据Vue.js的版本和构建工具的不同而有所变化，因此您应该查看相应版本的官方文档以获取详细信息。

### 示例

```vue
const path = require('path');

module.exports = {
  // 设置开发服务器的配置
  devServer: {
    port: 8082, // 设置开发服务器端口
    open: true, // 自动在浏览器中打开应用程序
    proxy: {
      '/api': {
        target: 'http://localhost:8083', // 后端服务器的地址（端口8083）
        changeOrigin: true,
        pathRewrite: {
          '^/api': '', // 去掉/api前缀，以与后端的路由匹配
        },
      },
    },
  },

  // 设置公共路径，如果您的应用程序部署在子路径中，需要设置
  publicPath: process.env.NODE_ENV === 'production' ? '/my-app/' : '/',

  // 设置构建输出目录
  outputDir: 'dist',

  // 配置Webpack，您可以添加自定义Loader、Plugin等
  configureWebpack: {
    resolve: {
      alias: {
        '@': path.resolve(__dirname, 'src'), // 设置别名以便在代码中引用时更简洁
      },
    },
  },

  // 开启Sourcemaps以便在浏览器中调试
  productionSourceMap: true,
};

```



# 单页面应用（SPA）

是一种现代的Web应用程序开发方式，其中整个应用程序通过JavaScript加载到浏览器，并在用户与应用程序进行交互时动态地更新页面内容，而不需要加载整个新页面。SPA通常是通过前端框架（如Vue.js、React、Angular等）实现的，具有以下特点：

1. **单页面加载**：SPA应用的初始加载只包含一个HTML文件，通常是一个空白页面，然后通过JavaScript动态加载所需的内容。这减少了初始加载时间，因为只有一次网络请求。

2. **路由导航**：SPA应用使用客户端路由来管理不同页面或视图之间的导航，而不是通过传统的页面刷新。这允许用户在应用内部无缝切换视图，而无需等待新页面加载。

3. **动态内容更新**：SPA应用通过AJAX或API请求从服务器获取数据，然后使用数据来更新页面内容，而不是重新加载整个页面。这提供了更快的用户体验，因为只有必要的内容被刷新。

4. **更丰富的用户体验**：SPA应用通常具有更多的交互性和动画效果，因为它们可以在客户端上执行更多的JavaScript代码。

5. **单页应用框架**：SPA通常是通过现代前端框架或库来实现的，这些框架提供了组件化、路由管理、状态管理等功能，以帮助开发者构建复杂的SPA应用。

6. **优点**：SPA可以提供更流畅的用户体验，因为它们减少了页面刷新和加载时间。它们还更容易实现应用的跨平台性，因为可以在Web、移动和桌面应用程序中共享代码。

7. **缺点**：SPA应用的初始加载可能会较慢，因为需要下载整个应用的JavaScript代码。此外，SPA应用对搜索引擎优化（SEO）通常需要更多的工作，因为内容是通过JavaScript生成的。

常见的SPA框架包括Vue.js、React、Angular等，它们提供了强大的工具来构建SPA应用。SPA应用适用于需要提供高度交互性、实时数据更新和更丰富用户体验的应用程序，如社交媒体平台、电子商务应用、在线工具等。







# JS

## Proxy

可以用于响应式对象

是 JavaScript 的一个内置对象，它允许你创建一个代理对象，用于拦截和定义对象上的基本操作，比如访问属性、赋值、删除属性等。使用 `Proxy` 可以对对象进行自定义的操作处理。

### 创建一个 Proxy 对象的基本语法：

```javascript
const proxy = new Proxy(target, handler);
```

- `target`：被代理的目标对象，可以是任何类型的对象（数组、函数、普通对象等）。
- `handler`：一个包含了拦截器的对象，定义了在目标对象上进行操作时的各种行为。

### 代理对象的拦截器（Handler）：

`handler` 对象包含了各种钩子（拦截器），用于拦截代理对象上的各种操作。常用的拦截器有：

- `get`：拦截对代理对象属性的访问操作。
- `set`：拦截对代理对象属性的赋值操作。
- `deleteProperty`：拦截删除代理对象属性的操作。
- `apply`：拦截对代理对象的函数调用操作。
- `construct`：拦截对代理对象的 `new` 操作。

### 示例：

```javascript
const target = {
  name: 'Alice',
  age: 25
};

const handler = {
  get(target, prop) {
    console.log(`Accessing ${prop}`);
    return target[prop];
  },
  set(target, prop, value) {
    console.log(`Setting ${prop} to ${value}`);
    target[prop] = value;
  }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // 输出：Accessing name，然后输出：Alice
proxy.age = 30; // 输出：Setting age to 30
```

在这个示例中，通过 `new Proxy()` 创建了一个代理对象 `proxy`。`handler` 中的 `get` 和 `set` 拦截器分别拦截了对代理对象属性的访问和赋值操作，并在控制台输出相应的信息。当访问 `proxy.name` 时，会触发 `get` 拦截器，并输出相应的信息。同样，赋值 `proxy.age = 30` 时，会触发 `set` 拦截器，并输出设置的信息。

`Proxy` 提供了对对象操作的强大控制和定制能力，能够帮助你实现更灵活的对象操作和处理逻辑。

## let

 是 JavaScript 中声明变量的关键字之一，它用于声明一个变量，并且允许你重新赋值这个变量的值。

### `let` 的基本用法：

```javascript
let variableName; // 声明一个变量，但不初始化，其初始值为 undefined
```

或者可以在声明时初始化变量：

```javascript
let variableName = 10; // 声明一个变量并初始化为 10
```

与 `var` 不同，`let` 声明的变量具有块级作用域。这意味着它们在声明它们的块（比如函数、循环或花括号 `{}` 内部）内部有效，而在块外部是不可访问的。

### 重新赋值变量：

使用 `let` 声明的变量可以重新赋值：

```javascript
let variableName = 10;
variableName = 20; // 可以重新赋值变量的值
```

### 示例：

```javascript
let num = 10; // 声明一个变量 num 并初始化为 10
if (true) {
  let num2 = 20; // 声明一个块级作用域的变量 num2 并初始化为 20
  console.log(num); // 输出：10
  console.log(num2); // 输出：20
}
console.log(num2); // 在这里会报错，因为 num2 在这个作用域外不可访问
```

在上面的示例中，`num` 使用 `let` 声明的变量在全局范围内有效。而 `num2` 使用 `let` 声明的变量在 `if` 块内部声明，只在该块级作用域内有效，块外部无法访问。