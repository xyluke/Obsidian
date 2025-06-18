![[../img/Pasted image 20250618101158.png]]

host的配置
https://blog.51cto.com/u_17135169/12571168

~~~ts
/*
 * Copyright (C) Microknown, 2022-2024 by kiterunner_t
 * TO THE HAPPY FEW
 */

import Taro from "@tarojs/taro";
import { Component } from "react";
import { View } from "@tarojs/components";
import { AtButton } from 'taro-ui';
import { Html5Qrcode } from 'html5-qrcode';
import "./ScanComponent.h5.less";

interface ScanProp {
    scanSuccess: Function
}
// 获取回调函数的方法

export default class ScanComponent extends Component<ScanProp, any> {
    qrCode: Html5Qrcode | null = null
    constructor(props) {
        super(props);
        this.state = {
            scanning: false,
            scanValue: ''
        }
    }

    startQrScanner = () => {
        this.setState({scanning: true}, () => {
            this.getCameras()
        })
    }


    stopQrScanner = () => {
        if (this.qrCode) {
            this.qrCode.stop().then(() => {
                this.setState({ scanning: false})
            }).catch((error) => {
                console.error('二维码扫描停止失败:', error)
            })
        }
    }


    start = () => {
        this.qrCode && this.qrCode.start(
            // environment后置摄像头 user前置摄像头
            { facingMode: "environment" },
            {
                fps: 20, // 可选，每秒帧扫描二维码
                qrbox: { width: 200, height: 200 }, // 可选，如果你想要有界框UI
                aspectRatio: 1.0 // 可选，视频馈送需要的纵横比，(4:3--1.333334, 16:9--1.777778, 1:1--1.0)传递错误的纵横比会导致视频不显示
            },
            (decodedText: any, decodedResult: any) => {
                // do something when code is read
                console.log('decodedText', decodedText)
                console.log('decodedResult', decodedResult)
                this.stopQrScanner();
                this.setState({scanValue: decodedText}, () => {
                    this.props.scanSuccess(decodedText);
                });
            },undefined
        )
        .catch((err: any) => {
            console.log('扫码错误信息', err)
            // 错误信息处理仅供参考，具体情况看输出！！！
            if (typeof err == 'string') {
            // this.$toast(err)
            } else {

            }
        })

    }

    getCameras = () => {
        Html5Qrcode.getCameras()
        .then((devices) => {
          if (devices && devices.length) {
            this.qrCode = new Html5Qrcode("qr-reader")
            this.start()
          }
        })
        .catch((err:any) => {
          // handle err
          this.qrCode = new Html5Qrcode("qr-reader")
          // ('您需要授予相机访问权限')
        })
    }


    render() {
        return (
            <View className="scan-box">
            { this.state.scanning ?
                <View className="qr-code-scanner">
                    <View id="qr-reader" />
                    <AtButton onClick={this.stopQrScanner}>停止扫描</AtButton>
                </View>
                : ''
            }
            </View>
        )
    }
}
~~~

~~~less
.scan-box {
    position: relative;
    .qr-code-scanner {
        position: fixed;
        top: 0;
        left: 0;
        height: 100VH;
        width: 100VW;
        background-color: #000;
        z-index: 10000;
    }

    #qr-reader {
        width: 100%;
        height: 400PX;
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    .at-button {
        position: fixed;
        bottom: 10PX;
        left: 50%;
        transform: translateX(-50%);
        width: 50vw;
        height: 60PX;
        background-color: #fff;
        color: #000;
    }
}


~~~


~~~tsx
import ScanComponent from '@/components/ScanComponent';
<ScanComponent ref={this.scanRef} scanSuccess={(v) => this.scanSuccess(v)}/>
~~~

