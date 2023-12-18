[js图片压缩](https://blog.csdn.net/yzh648542313/article/details/128862111)
[图片压缩](https://blog.csdn.net/weixin_44727080/article/details/129247755)


~~~html
<div class="codepadding">
<el-upload
	ref="upload"
	action="https://api.microknown.com/api/portal.File/upload2"
	:on-preview="handlePreview"
	:on-remove="handleRemove"
	:file-list="fileList"
	:auto-upload="false"
	:on-success="setImgUrl"
>
	<el-button slot="trigger" size="small" type="primary">选取文件</el-button>
	<el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>
	<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
</el-upload>

<div style="height: 450px;width: 100%;">
	<div v-for="(item,index) in fileList" :key="index">{{(item.size/1024).toFixed(2)}}kb</div>
</div>
<el-button type="primary" @click="toZip">点击压缩</el-button>
</div>
~~~


~~~js
submitUpload() {
	this.$refs.upload.submit();
},


handleRemove(file, fileList) {
	console.log(file, fileList);
},


handlePreview(file) {
	console.log(file);
},


setImgUrl(response, file, fileList) {
	console.log(response, file, fileList);
	this.fileList = fileList
},


toZip() {
	this.fileList.forEach(item => {
		let pathPre = 'https://api.microknown.com:443/image';
		let url = pathPre + item.response.body.path;
		this.urlToBlob(url)
			.then(blob => {
				// 在这里可以对 Blob 进行操作，例如上传到服务器或展示在页面上
				console.log(blob);
				// this.compressImg(blob)
				this.compressedImage(blob, {height: 100, width: 100,quality: 1}, 504800, (value) => {
					console.log(value, '重置后的值');
					console.log(this.base64ToUrl(value));
				})
			})
			.catch(error => {
				console.error('发生错误：', error);
			});
		
	})
},


urlToBlob(url) {
	return fetch(url)
		.then(response => response.blob())
		.then(blob => blob);
},


base64ToUrl(base64) {
	console.log(base64);
	const arr = base64.split(",")
		// mime = arr[0].match(/:(.*?);/)[1]
		console.log(arr[1]);
	const byteCharacters = atob(arr[1]); // 解码 base64 字符串
	const byteArrays = [];

	for (let i = 0; i < byteCharacters.length; i++) {
		byteArrays.push(byteCharacters.charCodeAt(i));
	}

	const byteArray = new Uint8Array(byteArrays);
	const blob = new Blob([byteArray], { type: 'image/jpeg' }); // 根据实际情况设置 MIME 类型

	return URL.createObjectURL(blob);
},


getBase64ImageSize(base64) {
    const indexBase64 = base64.indexOf('base64,');
    if (indexBase64 < 0) return -1;
    const str = base64.substr(indexBase64 + 6);
    // 大小单位：字节
    return (str.length * 0.75).toFixed(2);
},


/**
 * 	图像压缩，默认同比例压缩
 * @param {Object} imgPath
 *	图像base64编码字符串或图像可访问路径
 * @param {Object} obj
 *	obj 对象 有 width， height， quality(0-1)
 * @param {Object} maxSize
 *	指定压缩后的文件大小，单位：字节
 * @param {Object} callback
 *	回调函数有一个参数，base64的字符串数据
 */
 compressedImage(imgPath, obj, maxSize, callback) {
     const reader = new FileReader();
    reader.readAsDataURL(imgPath);

    reader.onload = () => {
        let img = new Image();
        img.src = reader.result;;
        img.onload = () => {
            const that = this;
            // 默认按比例压缩
            let w = img.width,
                h = img.height,
                scale = w / h;
            w = obj.width || w;
            h = obj.height && obj.height * (w / scale) || h;
            // 生成canvas
            let canvas = document.createElement('canvas');
            let ctx = canvas.getContext('2d');
    
            canvas.width = w;
            canvas.height = h;
    
            ctx.drawImage(img, 0, 0, w, h);
            // 图像质量，默认图片质量为0.8
            let quality = 0.8;
            if (obj.quality && obj.quality > 0 && obj.quality <= 1) {
                quality = obj.quality;
            }
            // quality值越小，所绘制出的图像越模糊
            let newBase64Image = canvas.toDataURL('image/jpeg', quality);
    
            let fileSize = this.getBase64ImageSize(newBase64Image);
            if (fileSize > maxSize && quality > 0.01) {
                if (quality > 0.05) {
                    quality = quality - 0.05;
                } else {
                    quality = 0.01;
                }
                this.compressedImage(imgPath, {
                    quality: quality
                }, maxSize, callback);
                return;
            }
    
            // 回调函数返回压缩后的 base64图像
            callback(newBase64Image);
        }
    
    }
}

~~~


其二
~~~js
/**
 * @压缩公共方法
 * @params file
 * @return 压缩后的文件，支持两种，file和 blob
 */
compressImg(file) {
	const reader = new FileReader();
	// readAsDataURL 方法会读取指定的 Blob 或 File 对象。读取操作完成的时候，readyState 会变成已完成DONE，并触发 loadend (en-US) 事件，
	// 同时 result 属性将包含一个data:URL格式的字符串（base64编码）以表示所读取文件的内容。
	reader.readAsDataURL(file);
	reader.onload = () => {
		const img = new Image();
		img.src = reader.result;
		// console.log(this.base64ToUrl(img.src, '66'));
			img.onload = () => {
				// 图片的宽高
				const w = img.width;
				const h = img.height;
				const canvas = document.createElement("canvas");
				// canvas对图片进行裁剪，这里设置为图片的原始尺寸
				canvas.width = w;
				canvas.height = h;
				const ctx = canvas.getContext("2d");
				// canvas中，png转jpg会变黑底，所以先给canvas铺一张白底
				ctx.fillStyle = "#fff";
				// fillRect()方法绘制一个填充了内容的矩形，这个矩形的开始点（左上点）在
				// (x, y) ，它的宽度和高度分别由width 和 height 确定，填充样式由当前的fillStyle 决定。
				ctx.fillRect(0, 0, canvas.width, canvas.height);
				// 绘制图像
				ctx.drawImage(img, 0, 0, w, h);
		
				// canvas转图片达到图片压缩效果
				// 返回一个包含图片展示的 data URI base64 在指定图片格式为 image/jpeg 或 image/webp的情况下，
				// 可以从 0 到 1 的区间内选择图片的质量。如果超出取值范围，将会使用默认值 0.92。其他参数会被忽略。
				const dataUrl = canvas.toDataURL("image/jpeg", 0.01);
				this.dialogImageUrl = dataUrl


				// base64格式文件转成Blob文件格式
				let blobFile = this.dataURLtoBlob(dataUrl);
				console.log("压缩后的图片：Blob文件----------");

				console.log(blobFile, blobFile.size/1024);
				// base64格式文件转成file文件格式
				let fileName = '555'
				
				let fileImg = this.dataURLtoFile(dataUrl,fileName);
				console.log("压缩后的图片：file文件----------");
				console.log(fileImg, fileImg.size/1024);
				this.compressFile = fileImg
			};
		};
},
	// canvas生成的格式为base64，需要进行转化, base64->file
	dataURLtoFile(dataurl,fileName) {
		let arr = dataurl.split(','),
		mime = arr[0].match(/:(.*?);/)[1],
	
		bstr = atob(arr[1]),
		n = bstr.length,
		u8arr = new Uint8Array(n);
		while (n--) {
			u8arr[n] = bstr.charCodeAt(n);
		}
		return new File([u8arr], fileName, {type:mime})
	},

	// canvas生成的格式为base64，需要进行转化, base64->blob
	dataURLtoBlob(dataurl) {
		const arr = dataurl.split(","),
			mime = arr[0].match(/:(.*?);/)[1],
			bstr = atob(arr[1]);
		let n = bstr.length;
		const u8arr = new Uint8Array(n);
		while (n--) {
			u8arr[n] = bstr.charCodeAt(n);
		}
		
		let blob = new Blob([u8arr], { type: mime });

		console.log(URL.createObjectURL(blob));
		return blob
	},

	base64ToUrl(base64) {
		console.log(base64);
		const arr = base64.split(",")
			// mime = arr[0].match(/:(.*?);/)[1]
			console.log(arr[1]);
		const byteCharacters = atob(arr[1]); // 解码 base64 字符串
		const byteArrays = [];

		for (let i = 0; i < byteCharacters.length; i++) {
			byteArrays.push(byteCharacters.charCodeAt(i));
		}

		const byteArray = new Uint8Array(byteArrays);
		const blob = new Blob([byteArray], { type: 'image/jpeg' }); // 根据实际情况设置 MIME 类型

		return URL.createObjectURL(blob);
	},
~~~

> 上面可以控制一个图片的压缩过程

