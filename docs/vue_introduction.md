# Vue.js
## Introduction
### What is Vue.js?
>Vue is a progressive framework for building user interfaces.

>The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects.

Vue 是一套用於建構用戶界面的漸進式框架。

Vue 的核心 library 只關注於 view layer，不僅容易上手，也便於和第三方 libraries 或是已有的 projects 結合。

---
### Getting Started
首要步驟為引入 Vue.js

```html
<!-- development version, includes helpful console warnings --><script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

or

```html
<!-- production version, optimized for size and speed --><script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
---
### Declarative Rendering

>At the core of Vue.js is a system that enables us to declaratively render data to the DOM using straightforward template syntax.

使用 app 將 data 渲染進入 DOM 之中。

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

接著我們將會看見
```js
Hello Vue!
```
在我們成功創建 Vue app 之後，現在 data 和 DOM 已經建立起了關聯，也就是之後我們可以使用資料來驅動畫面，且所有東西都成為響應式 (reactive)。
我們來看看官網如何示範綁定元素 attribute:

```html
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
```

範例中使用 `v-bind` ，用來使這項元素的節點的 `title` `attribute` 和 Vue 實例中的 `message` 的屬性相同。

---
### Conditionals and Loops

>It’s easy to toggle the presence of an element, too
以下會分別介紹： `v-if` 以及 `v-for`：

- `v-if` 這項指令可使用條件來控制元素的切換

    ```html
    <div id="app-3">
      <span v-if="seen">Now you see me</span>
    </div>
    ```

    ```js
    var app3 = new Vue({
      el: '#app-3',
      data: {
        seen: true
      }
    })
    ``` 
  
    接著我們能很容易的看到這一行文字 
  
    ``` 
    Now you see me
    ```

    而若使用 `app.seen = false` ，則這行文字便會消失。

- `v-for` 這指令可以綁定 data 來渲染 a list of items

    ```html
    <div id="app-4">
      <ol>
        <li v-for="todo in todos">
          {{ todo.text }}
        </li>
      </ol>
    </div>
    ```

    ```js 
    var app4 = new Vue({
      el: '#app-4',
      data: {
        todos: [
        { text: 'Learn JavaScript' },
        { text: 'Learn Vue' },
        { text: 'Build something awesome' }
        ]
      }
    })
    ```

    ```js
    1.Learn JavaScript 
    2.Learn Vue 
    3.Build something awesome 
    ```

    如果想繼續新增，可使用 `app.todo.push{{ text: '我是新項目' }}` 來加增新項目。
    
---
### Handling User Input
>To let users interact with your app, we can use the v-on directive to attach event listeners that invoke methods on our Vue instances.

以下會分別介紹： `v-on` 以及 `v-model`：

- `v-on` 我們可以使用 `v-on` 這項指令來監聽事件 
    ```html 
    <div id="app-5">
      <p>{{ message }}</p>
      <button v-on:click="reverseMessage">Reverse Message</button>
    </div>
    ```

    ```js 
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

     而這部分官網有特別註明一段文字 
     
     >Note that in this method we update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.
     
    指當我們在更新狀態時，並沒有碰觸到 DOM，因為這些已經由 Vue 處理完畢，我們只需要專注於邏輯層面即可。

- `v-model` 我們可以使用 `v-model` 這項指令來進行雙向綁定 。

    ```html 
    <div id="app-6">
      <p>{{ message }}</p>
      <input v-model="message">
    </div>
    ```

    ```js 
    var app6 = new Vue({
      el: '#app-6',
      data: {
        message: 'Hello Vue!'
      }
    })
    ```

    如此一來，當我們改變 input 輸入內容的同時，畫面狀態也會同時改變。
    
---
### Composing with Components
    
>The component system is another important concept in Vue, because it’s an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components.

組件系統可以讓我們使用小型、獨立和可重複使用的組件來建構大型的 applications。

```js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue(...)
```

小型的 component 製作好之後，也可以用它來建構另一個 component 模板。

```html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

而這父層和子層之間共同需要的 data 不需要重複寫入，可以使用 `prop` 來傳送。

```js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

  如此一來， 便可以透過 `prop` 將 data 從父作用域傳送到子組件，且不會影響到父層。

綜合上述的指令，可以試著做出以下的範例：
```html
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```

```js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```

```html
1.Vegetables
2.Cheese
3.Whatever else humans are supposed to eat
```

在大型的應用中，有必要將整個 app 劃分為組件，開發更容易管理，例如以下：
```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### 總結
以上為 Vue.js 官網的簡介，也介紹了一些基本的功能和用法，希望能夠督促自己回頭常去看最原始的文件定義，以及以整體架構去了解，另一方面也是在一邊讀的同時做一些筆記，下次查詢時也能夠參考。接著下一篇文章就繼續進行製作 Vue 的實例囉。

#### Reference
- [Vue.js](https://vuejs.org/v2/guide/)