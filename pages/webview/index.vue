<template>
	<view class="webview">
		<web-view ref="webview" :src="url" @onPostMessage="handleMessage" @message="handleMessage"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: "/hybrid/html/index.html"
			}
		},

		onLoad() {
			this.setStatusbar()
		},
		methods: {
			setStatusbar() {
				let height = 0; //定义动态的高度变量
				let statusbar = 0; // 动态状态栏高度
				uni.getSystemInfo({ // 获取当前设备的具体信息
					success: (sysinfo) => {
						statusbar = sysinfo.statusBarHeight;
						height = sysinfo.windowHeight;
					}
				});
				let currentWebview = this.$scope.$getAppWebview(); //获取当前web-view
				setTimeout(function() {
					var wv = currentWebview.children()[0];
					wv.setStyle({ //设置web-view距离顶部的距离以及自己的高度，单位为px
						top: statusbar, //此处是距离顶部的高度，应该是你页面的头部
						height: height - statusbar
					})
				}, 200); //如页面初始化调用需要写延迟

			},

			//接收webview传递来的消息
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
					
						this.currentWebview = this.$mp.page.$getAppWebview()
						this.currentWebview.children()[0].evalJS(`scanCode('${query}')`)
					}
				})
			}

		}
	}
</script>

<style>
	.webview {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>