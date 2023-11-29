
~~~js
    h5: {
        publicPath: '/',
        staticDirectory: 'static',
        esnextModules: ['taro-ui'],
        
        output: {
            filename:'js[hash:3]/[hash:10].js',
            chunkFilename: `chunk[hash:3]/[name].[chunkhash].[hash:4].js`
        },
        miniCssExtractPluginOption: {
            filename: 'css[hash:3]/[name].[hash].css',
            chunkFilename: 'css[hash:3]/[name].[chunkhash].[hash:5].css',
        },
        
        postcss: {
            autoprefixer: {
                enable: true,
                config: {
                    browsers: ['last 3 versions', 'Android >= 4.1', 'ios >= 8']
                }
            },
            "postcss-px-scale": {
                enable: true,
                config: {
                    scale: 0.5,
                    units: 'rpx',
                    includes: ['taro-ui']
                }
            },
            pxtransform: {
                enable: true,
                config: {
                  platform: 'h5',
                  // 这里设置640 也字体偏大
                  designWidth: 640,
                  deviceRatio: {
                    640: 2.34 / 2,
                    750: 640 / 750,
                    828: 1.81 / 2
                  },
                  /* pxtransform 配置项 */
                },
            },
            cssModules: {
                enable: false,
                config: {
                    namingPattern: 'module',
                    generateScopedName: '[name]__[local]___[hash:base64:5]'
                }
            },
        },
        devServer: {
            port: 8888
        },
        webpackChain (chain, webpack) {
            chain.resolve.extensions.add('.ts').add('.tsx');
            chain.module
              .rule('tsx')
              .test(/\.(tsx|ts)$/)
              .use('ts-loader')
              .loader('ts-loader')
              .options({
                  transpileOnly: true,
              });
        }
    }
~~~
> 上面文件夹加hash是让每次打包的文件夹名称不一样。移动端h5就现阶段不会出现线上更新了，线下还是旧页面数据的问题。

![[../img/Pasted image 20231129092230.png]]