<template>
	<view class="content">
		<button type="default" @click="toWebView()">通过方法跳转到about页面</button>
	</view>
</template>

<script>
	export default {
		methods: {
			toWebView(){
				console.log("2423423")
				uni.navigateTo({
					url:"/pages/webview/index"
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>