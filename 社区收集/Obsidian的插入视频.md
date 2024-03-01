~~~js
<iframe src="https://www.bilibili.com/video/BV1Um411S7SZ/?share_source=copy_web" scrilling="no" border="0" width="800" height="500" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

// 测试视频
iframe里的src拼上 https: 才能正常显示视频
如果想关闭弹幕，src拼上&danmaku=0
如果嫌默认窗口太小，可以加上自定义宽高属性 width=“800” height=“500”
width: 宽，可以写100%(显示为窗口的百分比)，也可以写数值，单位默认是px
height: 高
<iframe src="https://www.bilibili.com/video/BV1Um411S7SZ/?share_source=copy_web&danmaku=0" scrolling="no" border="0" width="800" height="500" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
~~~