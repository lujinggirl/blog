---
title: 工程化
date: 2021-01-26 16:38:33
tags:
---
<meta name="referrer" content="no-referrer"/>

## 1.Webpack如果使用了hash命名，是否每次都会重新生成hash?简单说下webpack的几种hash策略？
**Webpack hash分类**
* 输出的结果全部使用hash的情况
1. 每个文件都具有相同的hash值，因为hash是基于我们使用的所有源文件生成的。
2. 如果我们只是重新运行该构建，而不更改内容，则hash值不变。
3. 如果只更改了某一个文件内容，则会重新生成新的hash，并且所有生成捆绑的名称中都会存在新的hash值。
* 输出的结果全部使用chunkhash的情况
1. chunkhash是根据不同的入口进行依赖文件解析，构建对应的chunk(模块)，生成对应的hash.
2. 使用上来说：可以把一些公共库和程序入口文件区分开，单独打包构建，通过chunkhash生成hash.
3. 那么我们只要不改公共库内容，就可以保证hash不受影响，起到缓存的作用。
* 输出的结果全部使用contenthash的情况
1. 每一个文件名称都有唯一的hash值，hash值是根据文件内容计算出来的。
2. 当构建的文件内容发生改变的时候，hash就会改变，并且不会影响同一模块下其他文件.

总结： 并不是每次都会重新生成hash,主要是看用了那种hash策略。
* hash:粒度整个文件
* chunkhash: 粒度依赖入口文件
* contenthash： 粒度到每个文件内容。

## 2.说说Webpack的Runtime、Manifest代码的作用？
主要是管理所有模块的交互。
**Runtime**
Runtime主要是指浏览器运行时，webpack用来连接模块化的应用程序的所有代码。
runtime包含：在模块交互时，连接模块所需的加载和解析逻辑。包含浏览器中已加载模块的连接，以及懒加载模块的执行逻辑。
**Manifest**
在代码经过打包、编译之后，形如index.htlm文件，一些bundle文件和各种资源加载到浏览器中，是不是src目录下的文件结构都已经不存在了，那webpack是如何管理所有模块之间的交互呢？这就是manifest数据的由来。

当编译器（complier）开始执行、解析和映射应用程序时，他会保留所有模块的详细要点。这个数据集合层位manifest，当打包完成发送到浏览器时，会在运行时通过manifest解析加载这些模块。
无论选择哪种模块语法，那些import和require都已经转化为_webpack_require_方法，此方法指向模块标识符，通过使用manifest中的数据，runtime将能够查询模块标识符，检索背后对应的模块。

总结： 
runtime: 根据manifest中的数据来管理模块代码，主要是指模块交互时，连接模块所需的加载和解析逻辑。包括：已经加载到浏览器中的连接模块逻辑，以及尚未加载模块的延迟加载逻辑。
manifest： 记录了在打包过程中，各个模块之间的信息以及关联关系。


