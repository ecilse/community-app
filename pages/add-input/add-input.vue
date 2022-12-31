<template>
	<view>
		<uni-nav-bar left-icon="left" left-text="返回" title="标题" statusBar @clickLeft="goBack">
			<view class="flex justify-center align-center w-100 font-md">所有人可见<text class="iconfont icon-shezhi"></text></view>
		</uni-nav-bar>
		<textarea v-model="content" class="p-1 uni-textarea" placeholder="说一句话吧~" />
		
		<uploadImages v-show="imgList.length>0" ref="uploadImages" @changeImage="changeImage" :normailImageList="imgList"></uploadImages>
		
		<view class="p-2 fixed-bottom flex align-center">
			<view class="iconfont icon-caidan icon-bottom"></view>
			<view class="iconfont icon-huati icon-bottom"></view>
			<view class="iconfont icon-tupian icon-bottom" @click="bottomIconEvent('add-image')"></view>
			
			<view class="ml-auto bg-main flex justify-center align-center text-white rounded" style="width: 140rpx;height: 60rpx;">发送</view>
		</view>
	</view>
</template>

<script>
	import uniNavBar from "@/uni-ui/uni-nav-bar/uni-nav-bar.vue"
	import uploadImages from "@/component/common/upload-images.vue"
	export default {
		components: {
			uniNavBar,
			uploadImages
		},
		data() {
			return {
				content: "",
				imgList: []
			}
		},
		methods: {
			bottomIconEvent(e) {
				switch (e) {
					case 'add-image':
						this.$refs.uploadImages.chooseImage();
						break;
				}
			},
			// 返回上一层
			goBack() {
				uni.navigateBack();
			},
			// 清空内容
			dropDraft() {
				this.content = ""
				this.imgList = []
			},
			changeImage(imgList) {
				this.imgList = imgList;
				console.log(this.imgList);
			}
		},
		onLoad() {
			uni.getStorage({
				key: "add-input-content",
				success: res => {
					let result = res.data;
					if(result) {
						this.content = result.content
						this.imgList = result.imgList
					}
					
				},
				// 找不到记录走的是这个方法，不是走的成功回调返回为空消息
				// fail: err => {
				// 	console.log(err,"2");
				// }
			})
		},
		// 阻止返回
		onBackPress() {
			if(this.content.trim() || this.imgList.length > 0) {
				uni.showModal({
					content: "是否保存为草稿",
					confirmText: "保存",
					cancelText: "不保存",
					success: res => {
						if(res.confirm) {
							// 用户保存
							uni.setStorage({
								key: "add-input-content",
								data: {
									content: this.content,
									imgList: this.imgList
								},
								success: () => {
									// 清空内容并返回
									this.dropDraft()
									this.goBack()
								}
							})
						} else {
							// 不保存
							// 清除本地存储数据
							uni.removeStorageSync('add-input-content');
							this.dropDraft()
							this.goBack()
						}
					}
				})
				return true
			}
		}
	}
</script>

<style>
.icon-bottom {
	width: 75rpx;
	height: 50rpx;
	text-align: center;
	font-size: 50rpx;
}
</style>
