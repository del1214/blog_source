---
layout: 
title: Vue项目问题汇总
date: 2018-06-29 15:18:38
categories: 
- Vue
tags: 
- Vue
---



# v-for 与 key 配合使用
* key 需要是惟一的
* *频繁数据变化的数组key 不要绑定 index,会引发不渲染的问题*
* 返回值没有 id的用[shortid](https://github.com/dylang/shortid)一类的库生成 id 后使用 


```javascript
import shortid from 'shortid'
const array = [{a:1,b:2}, ...]
const newArray = array.map(item => {
	item._id = shortid.generate()
	return item
})

vm.array = newArray
```

# 理解configs/axios.js 配置

* `configs/axios.js` 中对 axios 做了请求和返回的封装
* `configs/axios.js` 的默认导出对象是对 cancelToken 的封装
* `configs/axios.js` 的默认导出对象与原始对象的 api 是有所区别的

# html 属性书写
* 优先写 Vue 指令
* Vue 指令按优先级顺序书写
* v-for 不应与 v-if 一起使用
* 绑定属性为字符串的应如此写 

	```javascript 
		:type="'Office'"
	```

* 静态的 html 属性放到最后写 

> 举个例子 ![举个例子](https://ws3.sinaimg.cn/large/006tKfTcly1fsrzbutigbj30km01b3yp.jpg)


# 善用计算属性
* 计算属性节约性能
* 计算属性属于函数式编程可测试

> 适用场景 

1. v-for 中需 v-if 过滤的
![v-for 中需 v-if 过滤的](https://ws2.sinaimg.cn/large/006tKfTcly1fsrzpmcarpj312z047q48.jpg)

2. v-if 中书写大量表达式的
![v-if 中书写大量表达式的](https://ws2.sinaimg.cn/large/006tKfTcly1fsrztj30ynj31940futf8.jpg)

# 常量提取

* 便于管理
* 便于修改

> 适用场景 
![](https://ws2.sinaimg.cn/large/006tKfTcly1fsrzwg9bo7j30mp0de41j.jpg)

![](https://ws1.sinaimg.cn/large/006tKfTcly1fsrzxy3mhbj30a50y7wiu.jpg)

# 拆解业务胶水代码

* 容易产生混乱的引用关系
* 前后数据依赖难以测试

> 适用场景 
![](https://ws4.sinaimg.cn/large/006tKfTcly1fss0wgmyrlj30o90nqtee.jpg)
![](https://ws2.sinaimg.cn/large/006tKfTcly1fss14nc1m4j30og0ilwht.jpg)


> 解决方案
> 
* 数组操作多使用 map 等返回新数组的操作, 少用 forEach
* 数组对象也可使用...操作符
* 将复杂的数据处理抽象成独立方法


# Mixin 按照规范编写

# 组件命名和使用 

# BAD 习惯
1. 克隆对象 克隆数组

```javascript
this.$emit('save', JSON.parse(JSON.stringify(this.multipleSelection)))
```

2. 没注释,注释不符合 [jsdoc](http://www.css88.com/doc/jsdoc/index.html) 标准

3. 单词拼写错误