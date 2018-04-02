##vue 学习之旅

### Vue.js 是什么?
是一套基于构建用户界面的渐进式框架

### 如何使用?
1. 创建项目vue-demo `npm init -y`   初始化
2. 安装webpack `npm install --save webpack`(方便别人clone项目,`npm i`可以安装需要的依赖所以`--save`安装)
3. 安装Vue  `npm i --save vue`
4. 新建`webpack.config.js`配置文件(用于压缩配置:输出路径等)
5. 新建`.gitignore`文件,为后期上传git准备,以防止把这个目录上传到github上.
6. 在package.json里链接仓库地址(可选)
```
  "repository": {
    "type": "git",
    "url": "https://github.com/FLYSASA/Hello-Vue.git"
  }
```
7. 链接github仓库,首先在远程创建好仓库. 
`git init`
`git remote add origin git@github.com:FLYSASA/Hello-Vue.git`

