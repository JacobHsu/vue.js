# 深入响应式原理 `Vue.set`

![Alt reactivity](https://cn.vuejs.org/images/data.png)

## 检测变化的注意事项

由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化。尽管如此我们还是有一些办法来回避这些限制并保证它们的响应性。

## 对于对象

Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property 必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的。例如：

```js
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的
```

对于已经创建的实例，**Vue 不允许动态添加根级别的响应式 property**

> 不能在直接data上增加屬性，可以在data裡的對象上增加屬性。 例如:`Vue.set(vm.someobj, {a,”b”})`

但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式 property。例如，对于：

`Vue.set(vm.someObject, 'b', 2)`

您还可以使用 `vm.$set` 实例方法，这也是全局 `Vue.set` 方法的别名：

`this.$set(this.someObject,'b',2)`

https://jsbin.com/kenekoh/edit?html,output

```js
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <script src="https://cdn.jsdelivr.net/vue/latest/vue.js"></script>
</head>
<body>
<div id="app">
    a is {{ a }},
    b is {{ b }},
    obj.b is {{ obj.b }}

</div>
 <script type="text/javascript">
    var vm = new Vue({
      el: '#app',
      data: {
        a: 1, //  `vm.a` 是响应式的
        obj: {}
      }
    });
    vm.b = 2 // `vm.b` 是非响应式的
    Vue.set(vm.obj, 'b', 3) // b = 2,  obj.b =3

</script>
</body>
```

有时你可能需要为已有对象赋值多个新 property，比如使用 `Object.assign()` 或 `_.extend()`。但是，这样添加到对象上的新 property 不会触发更新。在这种情况下，你应该用原对象与要混合进去的对象的 property 一起创建一个新的对象。

```js
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

### Vue 不能检测对象属性的添加或删除

```js
<div id="app">
    <p>{{obj}}</p>
</div>
<script>
  var vm = new Vue({
    el: '#app',
    data: {
      obj: {},
    },
    mounted() {
      //想要動態添加對象的屬性並且頁面能更新到
      this.obj.a = 1111  //瀏覽器什麼都沒渲染出來
      this.$set(this.obj,'a',1) // 方法一 { "a": 1 }
      this.obj = Object.assign({}, this.obj,{a:2})  //方法二 可以添加多個屬性 //{ "a": 2 }
    }
  })
</script>
```
https://juejin.im/post/5aa89ab9f265da238b7db328  


物件部分：[一開始沒有被註冊到的物件不會響應式更新](https://pjchender.blogspot.com/2017/05/vue-vue-reactivity.html)  

## 异步更新队列

Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部对异步队列尝试使用原生的 `Promise.then`、`MutationObserver` 和 `setImmediate`，如果执行环境不支持，则会采用 `setTimeout(fn, 0)` 代替。


例如，当你设置 `vm.someData = 'new value'`，该组件不会立即重新渲染。当刷新队列时，组件会在下一个事件循环“tick”中更新。多数情况我们不需要关心这个过程，但是如果你想基于更新后的 DOM 状态来做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员使用“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们必须要这么做。为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即使用 `Vue.nextTick(callback)`。这样回调函数将在 DOM 更新完成后被调用。例如：

全局 API [Vue.nextTick](https://cn.vuejs.org/v2/api/#Vue-nextTick)

```html
<div id="example">{{message}}</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

在组件内使用 `vm.$nextTick()` 实例方法特别方便，因为它不需要全局 Vue，并且回调函数中的 `this` 将自动绑定到当前的 Vue 实例上：

```js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '未更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '已更新'
      console.log(this.$el.textContent) // => '未更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})
```

因为 `$nextTick()` 返回一个 Promise 对象，所以你可以使用新的 ES2017 `async/await` 语法完成相同的事情：

```js
methods: {
  updateMessage: async function () {
    this.message = '已更新'
    console.log(this.$el.textContent) // => '未更新'
    await this.$nextTick()
    console.log(this.$el.textContent) // => '已更新'
  }
}
```
