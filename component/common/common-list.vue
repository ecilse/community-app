<template>
	<!-- 列表 -->
	<view class="p-2">
		<!-- 头部 头像&关注按钮 -->
		<view class="flex align-center justify-between">
			<!-- 头像和昵称 -->
			<view class="flex align-center">
				<image class="rounded-circle mr-2" :src="item.userpic" style="width: 65rpx;height: 65rpx;" @click="openSpace"></image>
				<view>
					<view class="font" style="line-height: 1.5;">{{item.username}}</view>
					<text class="font-sm text-light-muted" style="line-height: 1.5;">{{item.newstime}}</text>
				</view>
			</view>
			<!-- 关注按钮 -->
			<view
			class="flex align-center justify-center rounded text-white bg-main animated faster"
			style="width: 90rpx;height: 50rpx;"
			v-if="!item.isFollow"
			hover-class="headShake"
			@click="follow"
			>关注</view>
		</view>
		<!-- 标题 -->
		<view class="" style="font-size: 30rpx; margin: 10rpx 0;" @click="openDetail">{{item.title}}</view>
		<!-- 图片 -->
		<image :src="item.titlepic" v-if="item.titlepic" class="rounded w-100" style="height: 350rpx;" @click="openDetail"></image>
		<!-- 图标按钮 -->
		<view class="flex align-center">
			<view class="flex align-center justify-center flex-1" :class="{'text-main': item.support.type === 'support'}" hover-stay-time="100" @click="dosupport('support')">
				<text class="iconfont icon-dianzan mr-2"></text>
				<text>{{item.support.support_count > 0 ? item.support.support_count : '赞'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1" :class="{'text-main': item.support.type === 'unsupport'}" hover-stay-time="100" @click="dosupport('unsupport')">
				<text class="iconfont icon-cai mr-2"></text>
				<text>{{item.support.unsupport_count > 0 ? item.support.unsupport_count : '踩'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1" hover-class="text-main" hover-stay-time="100" @click="openDetail">
				<text class="iconfont icon-pinglun2 mr-2"></text>
				<text>{{item.comment_count > 0 ? item.comment_count : '评论'}}</text>
			</view>
			<view class="flex align-center justify-center flex-1" hover-class="text-main" hover-stay-time="100" @click="openDetail">
				<text class="iconfont icon-fenxiang mr-2" ></text>
				<text>{{item.share_num > 0 ? item.share_num : '分享'}}</text>
			</view>
		</view>
	</view>
</template>


<script>
	export default {
		props:['item', 'index'],
		methods:{
			// 进入个人中心
			openSpace() {
				console.log('进入个人中心')
			},
			// 关注
			follow() {
				this.$emit('follow', this.index)
			},
			// 进入详情页
			openDetail() {
				console.log('进入详情页')
			},
			// 点赞
			dosupport(dothing) {
				this.$emit('dosupport', {
					dothing,
					index: this.index
				})
			},
		}
	}
</script>

<style>
</style>