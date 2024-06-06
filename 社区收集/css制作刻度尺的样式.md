![[../img/Pasted image 20240606144941.png]]

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		.example{
			height: 30px;
			width: 400px;
			margin-top: 100px;
			background-image: repeating-linear-gradient(to right, #000000 0, #000000 0.05em, transparent 0, transparent 5em);
			/* repeating-linear-gradient(to right, #000000 0, #000000 0.05em, transparent 0, transparent 1em); */
			/* repeating-linear-gradient(to right, #000000 0, #000000 0.05em, transparent 0, transparent 1em),
			repeating-linear-gradient(to right, #000000 0, #000000 0.05em, transparent 0, transparent 1em);  */
			background-size: 100% 10px;
			background-repeat: no-repeat;
			background-position: 0.05em 100%;
			border-bottom: 1px solid #000000;
		}
	</style>
</head>
<body>
	<div class='example'></div>
</body>
</html>
~~~


~~~css
        .hour {
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            .item {
                flex: 1;
                position: relative;
                text-align: center;
                font-size: 10px;
                // border-right: 1px solid @borderColor;
            }
            .hour-text {
                position: absolute;
                top: 0px;
                left: -8px;
                width: 16px;
                text-align: center;
            }
            .hour-c {
                height: 100%;
                width: 100%;
                background-image: repeating-linear-gradient(to right, #000000 0, #000000 1px, transparent 0, transparent 100%),
                repeating-linear-gradient(to right, #00000080 0, #00000080 1px, transparent 0, transparent 25%);
                background-size: 100% 12px ,100% 5px;
                background-repeat: no-repeat;
                background-position: 0.0001em 100%;
                border-bottom: 1px solid #00000080;
            }

        }
/*html*/
<section v-else-if="hoursMode" class="hours">
	<div v-for="h in hours" :key="h" class="hour" style="width: 200px">
		{{h}}
	</div>
</section>
<selection class="hours" v-show="hoursMode">
	<div v-for="h in hours" :key="h" class="hour" style="width: 200px">
		<div v-for="hour in getHoursNumber" :key='hour' class="item hour-c">
			<span class="hour-text">
				{{hour}}
			</span>
		</div>
	</div>
</selection>
~~~


[参考文章](https://www.jb51.net/css/646909.html)
[文章2](https://zhuanlan.zhihu.com/p/467617116)
[文章3](https://cloud.tencent.com/developer/article/2367615)
