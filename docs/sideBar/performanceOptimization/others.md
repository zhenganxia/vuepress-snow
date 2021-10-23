### 代码优化
#### 1、代码模块化
常用的地方封装成单独的组件，分为工具类和组件类，在需要用到的地方引用，而不是写过多重复的代码，每一个组件都要明确含义，复用性越高越好，可配置型越强越好，包括咱们的css也可以通过less和sass的自定义css变量来减少重复代码。
#### 2、for循环设置key值
在用v-for进行数据遍历渲染的时候，为每一项都设置唯一的key值，为了让Vue内部核心代码能更快地找到该条数据，当旧值和新值去对比的时候，可以更快的定位到diff。
#### 3、Vue路由设置成懒加载
首屏渲染的时候，能够加快渲染速度
#### 4、理解Vue的生命周期
不要造成内部泄漏，使用过后的全局变量在组件销毁后重新置为null。
#### 5、使用keep-alive
keep-alive是Vue提供的一个比较抽象的组件，用来对组件进行缓存，从而节省性能。
#### 6、设计模式优化代码
享元模式、策略模式、状态模式优化if-else
#### 7、分批处理(time slicing)
下面这个性能优化的点是前端通用的，可以用requestAnimationFrame分批次执行大数据量的计算,防止一次性执行的数据太大从而阻塞页面渲染。
```js
fetchItems({ commit }, { items }) {
    commit('clearItems');
    commit('addItems', items)
}
改写：
fetchItems({ commit }, { items, splitCount }) {
    commit('clearItems');
    //新建一个队列
    const queue = new JobQueue();
    splitArray(items, splitCount).forEach(chunk => queue.addJob(done => {
        // 分片
        requestAnimationFrame(() => {
            commit('addItems', chunk);
            done()
        });
    }));
    
    // 等待所有数据处理完毕
    awiat queue.start();
}
```
#### 8、函数式编程
函数式编程利于webpack tree-shaking

#### 9、仅渲染可视化部分
vue-virtual-scroller这个组件，进行优化无限列表


### 打包优化
#### 1、修改vue.config.js中的配置项
把productionSourceMap设置为false，不然最终打包过后会生成一些map文件，如果不关掉，生成环境是可以通过map去查看源码的，并且可以开启gzip压缩，使打包过后体积变小。
#### 2、使用cdn的方式外部加载一些资源
比如vue-router、axios等Vue的周边插件-在webpack.config.js里面，externals里面设置一些不必要打包的外部引用模块。然后在入门文件index.html里面通过cdn的方式去引入需要的插件。
![avatar](/images/webpackExtral1.png)
![avatar](/images/webpackExtral2.png)
#### 3、减少图片使用，因为对于网页来说，图片会占用很大一部分体积，所以，优化图片的操作可以有效的来加快加载速度。
可以用一些css3的效果来代替图片效果，或者使用雪碧图来减少图片的体积
小于10000的图片转为base46
#### 4、按需引入，使用的一些第三方库可以通过按需引入的方式加载。
比如在使用element-ui库的时候，可以只引入需要用到的组件。


