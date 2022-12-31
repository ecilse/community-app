<template>
	<view>
		<!-- 导航栏 -->
		<uni-nav-bar fixed statusBar :border="false" @clickRight="intoAddInput">
			<view class="w-100 flex justify-center align-center">
				<view v-for="(item, index) in tabBar"
				:key="index" class=" font-md text-light-muted mx-1 font-weight-bold"
				:class="curTabBarIndex === index ? 'font-lg text-main' : ''"
				@click="changeTabbar(index)"
				>{{item.name}}</view>
			</view>
			<view slot="right">
				<text class="iconfont icon-fatie_icon"></text>
			</view>
		</uni-nav-bar>
		<!-- 动态 -->
		<swiper :duration="1000" :style="'height: '+scrollH+'px'">
			<!-- 关注 -->
			<swiper-item>
				<!-- 上下滚动 -->
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px'" @scrolltolower="loadmore(curTabBarIndex)">
					<block v-for="(item,index) in newsList.list" :key="index">
						<common-list :item="item" :index="index" @dosupport="dosupport"></common-list>
					</block>
					<load-more :loadmore="newsList.loadmore"></load-more>
				</scroll-view>
			</swiper-item>
			<!-- 话题 -->
			<swiper-item>
				<!-- 上下滚动 -->
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px'">
					<view class="flex align-center justify-between">
						<text>热门分类</text>
						<view class="flex align-center">
							更多<text class="iconfont icon-jinru"></text>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
			
		</swiper>
	</view>
</template>

<script>
	import uniNavBar from "../../uni-ui/uni-nav-bar/uni-nav-bar.vue"
	import commonList from "../../component/common/common-list.vue"
	import loadMore from "../../common/load-more.vue"
	let list = [
		{
			username: '昵称',
			userpic: '/static/default.jpg',
			newstime: '2019-07-10 下午 4:22',
			isFollow: true,
			title: '我是标题',
			titlepic: '/static/demo/demo2.jpg',
			support: {
				type: 'support',
				support_count: 1,
				unsupport_count: 2
			},
			comment_count: 2,
			share_num: 2
		},{
			username: '昵称',
			userpic: '/static/default.jpg',
			newstime: '2019-07-10 下午 4:22',
			isFollow: true,
			title: '我是标题',
			titlepic: '',
			support: {
				type: 'unsupport',
				support_count: 1,
				unsupport_count: 2
			},
			comment_count: 2,
			share_num: 2
		},{
			username: '昵称',
			userpic: '/static/default.jpg',
			newstime: '2019-07-10 下午 4:22',
			isFollow: true,
			title: '我是标题',
			titlepic: '/static/demo/demo2.jpg',
			support: {
				type: '',
				support_count: 1,
				unsupport_count: 2
			},
			comment_count: 2,
			share_num: 2
		}
	]
	export default {
		components:{
			uniNavBar,
			commonList,
			loadMore
		},
		data() {
			return {
				scrollH: 0,
				curTabBarIndex:0,
				tabBar:[{
					name: "关注"
				},{
					name: "话题"
				}],
				newsList: []
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - res.statusBarHeight - 44
					this.newsList = {
						loadmore: "上拉加载更多",
						list
					}
				}
			})
		},
		methods: {
			changeTabbar(index) {
				this.curTabBarIndex = index
			},
			intoAddInput() {
				uni.navigateTo({
					url: "/pages/add-input/add-input"
				})
			},
			// 点赞或踩
			dosupport({dothing, index}) {
				let item = this.newsList.list[index].support
				// 重复点赞或者踩
				if(dothing === item.type) return
				// 点过赞或踩（从多添加的里面扣除）
				if(item.type) {
					item[`${item.type}_count`]--
				}
				// 赞或踩的数量++;
				if(dothing === 'support') {
					item.type = 'support'
					item.support_count++
				}
				if(dothing === 'unsupport') {
					item.type = 'unsupport'
					item.unsupport_count++
				}
			},
			// 滚动到底部事件
			loadmore(curTabBarIndex) {
				let item = this.newsList
				// 不处于上拉加载更多就return出去
				if(item.loadmore !== '上拉加载更多') return
				item.loadmore = '加载中...'
				setTimeout(()=>{
					item.list = [...item.list, ...item.list]
					item.loadmore = '上拉加载更多'
				},2000)
			},
		}
	}
</script>

<style>

</style>
