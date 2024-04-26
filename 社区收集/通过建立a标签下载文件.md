~~~js
	  let a = document.createElement("a");//创建a标签
      a.setAttribute("href", ConfigBaseURL + "/downSong?id=" + id);//设置文件下载地址
      a.setAttribute('target', '_blank');//在当前页面创建下载
      document.body.appendChild(a);//添加到body
      a.click();//触发事件
      document.body.removeChild(a);//移除标签
~~~