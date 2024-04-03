---
title: Vue3再学习
published: 2023-10-04
tags: [Vue3, 前端工程化]
category: Vue3
---
## [Vue3再学习](https://vue3js.cn/es6/#正文)

### 前置知识

#### ES6的代理模式 | Proxy

##### 语法

- target：要使用Proxy包装的目标对象
- handler：以函数作为属性的对象，用于定制拦截行为

  ```js
  const proxy =new Proxy(target,handle)
  ```

  eg:

  ```js
  const origin = {}
  const obj = new Proxy(origin, {
    get: function (target, propKey, receiver) {
  		return '10'
    }
  });

  obj.a // 10
  obj.b // 10
  origin.a // undefined
  origin.b // undefined
  ```

  给空对象的get架设了一层代理，所有get操作都会返回定制的数字10，代理只会对Proxy对象生效，对origin就没作用。

##### handler.get

=> `get(target, propKey, ?receiver)`

eg:

```js
const person = {
  like: "vuejs"
}

const obj = new Proxy(person, {
  get: function(target, propKey) {
    if (propKey in target) {
      return target[propKey];
    } else {
      throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
    }
  }
})

obj.like // vuejs
obj.test // Uncaught ReferenceError: Prop name "test" does not exist.
```

    上面的代码表示在读取代理目标的值时，如果有值则返回，没有值就抛出自定义错误

   注：

- 访问的目标属性是不可写以及不可配置的，则返回的值必须与该目标属性的值相同
- 如果要访问的目标属性没有配置访问方法，即get方法是undefined的，则返回值必须为undefined

##### 可撤销Proxy
