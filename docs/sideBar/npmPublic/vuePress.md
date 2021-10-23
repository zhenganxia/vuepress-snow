[vuePress官网](https://vuepress.vuejs.org/zh/guide/getting-started.html)
:::warning
VuePress 需要 Node.js (opens new window)>= 8.6
:::
### 安装vuePress
1.创建并进入一个新目录
```js
git clone  vuepress-snow项目地址
```
2.初始化项目
```js
yarn init # npm init
```
3.将 VuePress 安装为本地依赖
```js
yarn add -D vuepress # npm install -D vuepress
```
4.package.js
```js
{
  "scripts": {
    "dev": "vuepress dev docs",
    "build": "vuepress build docs"
  }
}
```
5.本地启动服务器
```js
yarn docs:dev # npm run docs:dev
```
6.项目中设置.gitignore 忽略 node_modules/ docs/.vuepress/dist
7.设置.vuepress
> docs
>> .vuepress
>>> config.js\
>>> enhanceApp.js\
>>> components
>>> public

```js
mkdir docs && cd docs
mkdir .vuepress && cd .vuepress
touch config.js // 配置文件
touch enhanceApp.js //  客户端应用的增强(vue插件引用)
mkdir components // 全局组件
```

8.一级和多级导航栏
>.vuepress\
> nav

```js
cd config.js
themeConfig:{
     nav: [
      { text: "首页", link: "/" },
      { text: "Ajax跨域详解", link: "/nav/ajax/index.md" },
      {
        text: "关注工具",
        items: [
          {
            text: "UI框架",
            items: [
              { text: "Element", link: "https://element.eleme.cn/#/zh-CN/component/installation" },
              { text: "Ant Design Vue", link: "https://2x.antdv.com/components/overview-cn/" },
            ],
          },
          {
            text: "JS",
            items: [
              { text: "ES6", link: "https://es6.ruanyifeng.com/" },
              { text: "ES2020", link: "https://www.cnblogs.com/mengfangui/p/13885589.html/" },
              { text: "TS", link: "https://typescript.bootcss.com/" },
            ],
          },
        ],
      },
    ],
}

```
9.一级和多级侧边栏设置
>.vuepress\
>sideBar
>>command
>>>git.md

```js
cd config 
themeConfig:{
    sidebar:[
        {
        title: "常用命令",
        collapsable: true,
        children: [
          { title: "git", path: "/sideBar/command/git" }
        ],
      },
    ]
}
```
### 配置gitpage
1.创建deploy-gh.sh
```js
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run build

# 进入生成的文件夹
cd docs/.vuepress/dist

# 如果是发布到自定义域名
git init
git add -A
git commit -m 'deploy'

# 把下面的 <USERNAME> 换成你自己的 Github用户名（这里是本项目打包后存储的仓库地址）
git push -f git@github.com:zhenganxia/vuepress-snow-page.git master

cd -
```
2.修改config.js
```js
base: "/vuepress-snow-page/", // 当前项目地址是vuepress-snow压缩包放的仓库名称
注意不要设置dest（会导致build生成文件路径不在docs中，默认是在docs中生成）
```
3.修改package.json-scripts
```js
"deploy": "npm run build && bash deploy-gh.sh"
```
4.执行成功-查看git上项目setting-pages查看关联情况，关联成功-通过生成地址可以直接访问vuePress项目了
注意：如果没有数据查看source对应的分支是否对
![avatar](/images/gitPage.png)
<!-- <img :src="$withBase('/images/gitPage.png')" alt="gitPage"> -->

### [博客搭建element](https://www.jianshu.com/p/93c532cdf951)