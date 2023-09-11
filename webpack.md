webpack重点5个配置

entry： 入口文件  相对路径

input : 输出文件   绝对路径（需要引入path）

loader： 加载器

plugin： 插件

mode： 生产模式和开发模式



webpack默认只能处理js文件遇到其他文件需要依赖于loader



```javascript
module:{
	rule: [
		{
			test: /\.s[ac]ss/,
			use: [
				"style-loader", // 将js中的css通过创建style标签添加到html文件中生效
				"css-loader",	// 将css资源编译成commonjs的模块到js中
				"sass-loader",  // 将sass和scss文件编译成css文件
			],
		},
	]
}
```

处理图片，webpack4需要引入file-loader和url-loader进行处理，而webpack5将这两个功能内置了webpack中，我们只需要简单配置即可处理图片资源

```javascript
module:{
	rule: [
		{
			test: /\.(jpe?g|png|gif|webp)$/,
			type: 'asset', // 会转换为base64
            parser: { // 配置将小于10kb的图片转为base64
                      // 用以减少网络请求
                	  // 但是图片体积会变大
                dataUrlCondition: {
                    maxSize: 10 * 1024;
                }
            }
		},
	]
}
```



想要将文件输出到不同的路径，可以将js输出的output中的filename加上/，例：static/js/main.js

图片的输出路径写在规则中，配置名为generator，配置项如下

```
{
			test: /\.(jpe?g|png|gif|webp)$/,
			type: 'asset',
            generator: {
            	// 输出图片名称
            	// [hash:10] hash取前十位
            	filename: "static/images/[hash][ext][query]"
            }
		},
```



每次重新打包都需要手动将之前的打包资源删除，可以在output配置项中增加`clean: true,`，自动清除上次打包的文件



打包字体文件的配置

```javascript
module:{
	rule: [
		{
			test: /\.(ttf|wolf2?)$/,
			type: 'asset/resource', // 不会转换为base64
            						// 注意图标不能转换为base64
            generator: {
            	// 输出图片名称
            	// [hash:10] hash取前十位
            	filename: "static/images/[hash][ext][query]"
            }
		},
	]
}
```



打包音视频的配置  ，这些资源并不需要额外处理，直接打包输出就可以，通过与字体文件相同的配置输出。

```javascript
module:{
	rule: [
		{
			test: /\.(ttf|wolf2?|mp[34]|avi)$/, // 直接将需要匹配的文件加入正则规则中
			type: 'asset/resource', 
            generator: {
            	// 输出图片名称
            	// [hash:10] hash取前十位
            	filename: "static/images/[hash][ext][query]"
            }
		},
	]
}
```

