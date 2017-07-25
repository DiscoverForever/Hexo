---
title: Vue数据双向绑定原理
date: 2017-07-25 10:50:36
tags: 【VueJS, Vue, 数据, 双向绑定】
---
## Vue监听数据变化原理
> 话不多说上代码
```javascript
  let data = {name: 'tyy'};
  observe(data);
  /**
   * 遍历源数据
   * @param obj
   */
  function observe(obj) {
    if (!obj || typeof obj !== 'object') return;
    for (let key in obj) {
      defineReactive(obj, key, obj[key]);
    }
  }
  /**
   * 监听数据
   * @param obj
   * @param key
   * @param val
   */
  function defineReactive(obj, key, val) {
    // 监听对象的子对象的数据变化
    observe(val);
    Object.defineProperty(obj, key, {
      get: () => {
        return val;
      },
      set: (newValue) => {
        val = newValue;
        console.log('i am change')
      },
      writeable: true, // 是否可写
      enumerable: true, // 是否可枚举
      configurable: false // 是否递归
    }
```
[参考博客:https://segmentfault.com/a/1190000004346467](https://segmentfault.com/a/1190000004346467)