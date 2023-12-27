# 场景通过

![[../../img/Pasted image 20231227104208.png]]

~~~js

wx.login({
	success(res) {
		console.log(res);
		request({
			url: 'https://api.weixin.qq.com/sns/jscode2session',
			data:{
			'js_code': res.code, 
			'grant_type': 'authorization_code', 
			'appid': 'wx15b545a3e6ddb289', 
			secret: '5aefd351789fce6bc8aab8174a196c43'},
			method:'GET',
			success: (rs) => {
				console.log(rs);
			}
		})
	}
})
~~~

[相关文章](https://developers.weixin.qq.com/miniprogram/dev/OpenApiDoc/user-login/code2Session.html)
