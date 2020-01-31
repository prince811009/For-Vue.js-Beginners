# Vue.js
## Template Syntax
>Vue.js uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying Vue instance’s data.

Vue.js 使用了基於 HTML 的模板語言，允許開發者將 DOM 綁定至底層 Vue 實例的數據。

---
### Interpolations
#### Text
綁定數據最常見的形式為使用雙括號的 text interpolation 。

```html
<span>Message: {{ msg }}</span>
```

在被綁定的數據對象上，如果 `msg` 屬性有改變，則內容也會更新。另外還有一種一次性的執行插值，可以透過 `v-once` 指令執行。

```html
<span v-once>This will never change: {{ msg }}</span>
```

#### Raw HTML
雙括號會將內部的數據解釋為普通的文本，若要使用真正的 HTML ，需要用到 `v-HTML` 指令。

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

不過官網並不建議這麼做，因為動態的渲染任意 HTML 可能很容易導致 XSS 攻擊，請只對可信任的內容使用 HTML 插值，絕不要對用戶提供的內容使用插值。

>Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to XSS vulnerabilities. Only use HTML interpolation on trusted content and never on user-provided content.

#### Attribute
欲作用在 HTML attribute 上，可使用 `v-bind` 指令。

```html
<div v-bind:id="dynamicId"></div>
```

對於布林 attribute 而言 ，其存在就表示為 `true` ，而 `v-bind` 相較之下就有些不同。

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果當 `isButtonDisabled` 的值為 `null` 、 `undefined` 、 `false` ，則 `disabled` 這項 attribute 不會被包含在渲染出來的 `<button>` 這個 element 中。


#### Using JavaScript Expressions
>Vue.js actually supports the full power of JavaScript expressions inside all data bindings.

對於所有的數據綁定， Vue.js 都提供了完整的 JavaScript 表達式支持，如下：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

上述這些表達式會在所屬的 Vue 實例的作用域下被解析，不過，每個綁定都只能包含單個表達式，因此例如以下的例子則不會正確執行。

```html
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```

---
### Directives
>Directives are special attributes with the v- prefix.

>A directive’s job is to reactively apply side effects to the DOM when the value of its expression changes.

指令是帶有 `v-` 前綴的特殊 attribute 。當表達式的直改變時，響應式的作用於 DOM 。

```html
<p v-if="seen">Now you see me</p>
```

例如使用 `v-if` 指令來根據表達式 `seen` 中的值來條件式的插入或移除元素。

#### Arguments
>Some directives can take an “argument”, denoted by a colon after the directive name. 

一個指令能夠接受一個參數，例如 `v-bind` 指令可以用於響應式的更新 HTML attribute，而這邊的 `href` 為參數，用來告知 `v-bind` 指令將元素的 `href` attribute 與表達式 `url` 的值綁定。

```html
<a v-bind:href="url">...</a>
```

另一個例子為使用 `v-on` 指令來監聽 DOM 事件。

```html
<a v-on:click="doSomething">...</a>
```

#### Dynamic Arguments
>Starting in version 2.6.0, it is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets.

在 2.6.0 之後，可使用中括號將 JavaScript 的表達式括號起來，作為指令的參數。

```html
<!--
Note that there are some constraints to the argument expression, as explained
in the "Dynamic Argument Expression Constraints" section below.
-->
<a v-bind:[attributeName]="url"> ... </a>
```

這其中中括號裡的 `attributeName` 會被 JavaScript 的表達式進行動態求值，最後求出來的值會被作為最終的參數使用。就像是如果我們的 Vue 實例中有一個 `data` 屬性 `attributeName` ，且值為 `"href"` ，那這項綁定就等價於 `v-bind:href` 。

同理，也可以用在動態的事件綁定處理函數中，來作為一個動態參數。例如以下範例，當 `eventName` 的值為 `"focus"` 時， `v-on:[eventName]` 等價於 `v-on:focus` 。

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

#### Modifiers
>Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the .prevent modifier tells the v-on directive to call event.preventDefault() on the triggered event.

修飾符為以 `.` 為開頭的特殊後綴，用來表示一個指令應該以特殊方式綁定。例如， `.prevent` 修飾符告知 `v-on` 指令對於觸發的事件調用 `event.preventDefault()` 。

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

---
### Shorthands
#### `v-bind` Shorthand

```html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

#### `v-on` Shorthand

```html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```