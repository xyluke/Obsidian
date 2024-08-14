
![[../img/Pasted image 20240814091654.png]]

谷歌是chrome://flags/
edge是edge://flags/

# iNotify的使用
~~~text
https://wangchujiang.com/iNotify/#t1%E6%B5%8B%E8%AF%95%E4%BE%8B%E5%AD%90
~~~

~~~text
npm i @wcjiang/notify
// 根据文档就能使用了
~~~

![[../img/Pasted image 20240814092052.png]]

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="app">
 
        <button onclick="showMsg()">显示消息</button>
        <button onclick="stopMsg()">结束消息</button>
    </div>
    <script>
        function sendNotification() {
            let noticeImg = 'https://pic1.zhuanstatic.com/zhuanzh/50b6ffe4-c7e-4317-bc59-b2ec4931f325.png'

            let notice = new Notification('Microknown通知', {
                body: '已经错过的风景就不要再打听了，当你选定了一个方向，另一边的风景便与你无关了。',
                icon: 'https://cloud.microknown.com/themeImageFile/favicon.ico',
                image: 'https://img2.baidu.com/it/u=1028011339,1319212411&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=313',
                tag: 1,
                data: {
                    url: 'https://juejin.cn'
                }
            })
            notice.onshow = function () {
                console.log('show')
                //setTimeout(notice.close.bind(notice), 5000) //五秒后自动关闭
            }
            notice.onclick = function (value) {
                let url = value.target.data.url;
                window.open(url, "__blank")
            }

            notice.onclose = function () {
                console.log('close')
            }

            notice.onerror = function (error) {
                console.log('error', error)
            }
        }
        if (window.Notification.permission === 'default'){
            window.Notification.requestPermission((res) => {
                console.log('Notification.requestPermission', res)
                sendNotification();
            })
            console.log(window.Notification.requestPermission);
        }
    </script>
</body>
</html>
~~~