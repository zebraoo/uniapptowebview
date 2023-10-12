# uniapptowebview
uniapp app项目与uniapp h5项目通过webview交互

h5项目
1、首先导入uni.webview.1.5.4.js后引用
```vue
import * as wv from '@/libs/uni.webview.1.5.4.js'
Vue.prototype.$wv = wv
```
2、h5向app发送消息
this.$wv.webView.postMessage({
					data: {
						action: 'scanCode'
					}
				})

3、h5接收app发来的消息
mounted() {
			uni.hideTabBar()
			this.tabbarIndex = this.index
			
			window.scanCode = (query)=>{
				const intPos = query.indexOf(':')
				const endPos = query.indexOf('.')
				const connectorId = query.slice(intPos + 3, endPos)
				
				console.log('扫码 connectorId ', connectorId)
				
			}
		},

app项目
1、引入webview
url: "/hybrid/html/index.html"
<web-view ref="webview" :src="url" @onPostMessage="handleMessage" @message="handleMessage"></web-view>
2、app接收h5传递来的消息
handleMessage(msg) {
				console.log("handleMessage--->", msg.detail.data)
				let action = msg.detail.data[0].action
				console.log('action--->',action)
				switch (action) {
					case 'scanCode':
						this.toScanCode()
						break
				}

			},

toScanCode() {
				console.log("toScanCode")
				uni.scanCode({
					success: (res) => {
						const query = decodeURIComponent(res.result)
					  3、app向h5发送消息
						this.currentWebview = this.$mp.page.$getAppWebview()
						this.currentWebview.children()[0].evalJS(`scanCode('${query}')`)
					}
				})
			}
  
