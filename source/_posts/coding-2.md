---
title: vue项目下props传进去的数据,生命周期勾子函数包括watch不触发的解决办法
tags: Vue
categories: 编程
---
vue项目下props传进去的数据,生命周期勾子函数包括watch不触发的解决办法
@[TOC](vue项目下props传进去的数据,生命周期勾子函数包括watch不触发的解决办法)

## 遇到的问题
 在深层props过程中，props的数据传到了目标文件 但却没有触发数据更新及页面更新；
 watch代码如下：
```javascript
  watch: {
  uploaConfig(newVal,oldVal){
   if (this.uploadConfig.moreList && this.uploadConfig.moreList.length > 0) {
      	this.moreList = newVal.moreList
      	}
  	}
  },
```

vue-devToola数据传递结果如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190131173426486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NTY4OA==,size_16,color_FFFFFF,t_70)
#### 方案解决过程一
考虑到使用了对象传递 watch可能无法检测到深层key属性变化，代码改成如下:
```javascript
 watch: {
	 'uploaConfig.moreList': {
	      handler (newVal) {
	      if (this.uploadConfig.moreList && this.uploadConfig.moreList.length > 0) {
	      	this.moreList = newVal.moreList
	      	}
	      },
	      deep: true
	    }
  	},
```

结果显而易见 还是不行；

#### 方案解决过程二
查阅: [vue官方文档](https://cn.vuejs.org/v2/api/#watch).得知如果是想watch检测到值变化并且立刻使用则需要加上 immediate: true,
```javascript
watch: {
	    'uploaConfig.moreList': {
	      handler (newVal) {
	      if (this.uploadConfig.moreList && this.uploadConfig.moreList.length > 0) {
	      	this.moreList = newVal.moreList
	      	}
	      },
	      deep: true,
	      immediate: true,
	    }
    }
```
最后博主问题终于得到解决了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190131175046627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDc1NTY4OA==,size_16,color_FFFFFF,t_70)

## 总结:
出现问题尽量先找官网 首先确定是自己没有了解到官方api的正确使用或者是一些特定解决方案，如若对您有帮助，麻烦点个赞吧~
## 感谢~
谢谢大家 麻烦给个关注 ^ _ ^
