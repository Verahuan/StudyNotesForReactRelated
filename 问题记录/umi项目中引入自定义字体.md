## 在 UmiJS 中打包与加载自定义字体

## 错误描述

首先使用@font-face的方法引入会显示找不到当前模块

将字体文件放进public文件夹之后可以不报错，但是字体文件不生效，大概意思是没有loder，所以文件不起作用

上面的情况下，已经安装了file-loader,进行了配置，但是依然不生效

最后选择了删除缓存文件，问题解决，具体如下

## 解决方法

使用 Webpack 打包字体文件的时候需要使用 file-loader 来处理打包文件，在 UmiJS 3 中可通过配置文件中的 chainWebpack 函数来自定义 Webpack 的配置。

1. 当然首先你得先装上 [file-loader](https://webpack.js.org/loaders/file-loader/)

```
npm install --save-dev file-loader
```

1. 然后编辑 UmiJS 的配置文件 [./umirc.ts](https://umijs.org/config#chainwebpack) 中的 [chainWebpack](https://github.com/neutrinojs/webpack-chain)

```
export default defineConfig({
  // ...
  chainWebpack(config){
    config.module // 配置 file-loader
      .rule('otf')
      .test(/.otf$/)
      .use('file-loader')
      .loader('file-loader');
  },
});
```

1. 然后在 less 样式文件中申明字体

```
@font-face {
  font-family: 'Customized Font';
  src: url('path/to/font.otf');
}
```

1. 最后在样式中使用申明好的字体

```
.text-with-customized-font-face {
    font-family: 'Customized Font', 'sans-serif';
}
```

**完成以上操作之后，依然没有反应，删除umi的缓存文件，上次CSS和Dva的问题全都是因为umi的缓存，下次一定要检查这个问题**

## 参考

umi的issue:https://github.com/umijs/umi/issues/1802

优秀博客：https://atlassc.net/2020/12/29/bundle-font-face-in-umijs/