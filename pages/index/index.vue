<template>
	<view class="indexContent">
		<!-- 导航栏 --><scroll-view
		class="scroll-row" scroll-x
		:scroll-into-view="scrollInto"
		scroll-with-animation
		style="height: 100rpx;"
		>
			<view
			v-for="item in scrollItems" :key="item.id"
			class="scroll-row-item py-3 px-2 font-md"
			:id="'tab' + item.id"
			:class="item.id === currentId ? 'text-main font-weight-bold font-lg' : ''"
			@click="tabbarChange(item.id)"
			>{{item.name}}</view>
		</scroll-view>
		<!-- 左右切换 -->
		<swiper :duration="1000" :style="'height: '+scrollH+'px'" :current="currentId>>0" @change="swiperChange">
			<swiper-item class="swiper-item" v-for="(item,index1) in newsList" :key="index1">
					<!-- 上下滚动 -->
					<scroll-view scroll-y="true" :style="'height: '+scrollH+'px'"  @scrolltolower="loadmore(index1)">
						<template v-if="item.list.length > 0">
							<!-- 列表和分割线 -->
							<block v-for="(item2,index2) in item.list" :key="index2">
								<divider></divider>
								<common-list :item="item2" :index="index2" @follow="follow" @dosupport='dosupport'></common-list>
							</block>
							<!-- 上拉加载 -->
							<load-more :loadmore="item.loadmore"></load-more>
						</template>
						<!-- 没有文章 -->
						<template v-else>
							<view class="flex flex-column justify-center align-center py-5">
								<image src="@/static/common/nothing.png" style="width: 400rpx; height: 400rpx;"></image>
								<text class="font-md">什么都没有呢</text>
							</view>
						</template>
					</scroll-view>
			</swiper-item>
		</swiper>
		
	</view>
</template>

<script>
	// 文章列表
	import commonList from "@/component/common/common-list.vue"
	// 加载更多
	import loadMore from "@/common/load-more.vue"
	export default {
		components: {
			commonList,
			loadMore
		},
		data() {
			return {
				scrollInto: "tab000",
				currentId: "000",
				scrollItems: [{
					name: "关注",
					id: "000"
				},{
					name: "推荐",
					id: "001"
				},{
					name: "体育",
					id: "002"
				},{
					name: "热点",
					id: "003"
				},{
					name: "财经",
					id: "004"
				},{
					name: "娱乐",
					id: "005"
				},{
					name: "军事",
					id: "006"
				},{
					name: "历史",
					id: "007"
				},{
					name: "本地",
					id: "008"
				}],
				newsList:[],
				scrollH: 600
			}
		},
		onLoad() {
			uni.getSystemInfo({
				success: e => {
					// 计算滚动的高度为屏幕高度(不计算头底导航栏)
					this.scrollH = e.screenHeight - uni.upx2px(100)
				}
			})
			this.getData()
		},
		methods: {
			getData() {
				let arr = []
				let list = [
					{
						username: '昵称',
						userpic: '/static/default.jpg',
						newstime: '2019-07-10 下午 4:22',
						isFollow: false,
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
						isFollow: false,
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
						isFollow: false,
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
				for(let i = 0; i < this.scrollItems.length; i++) {
					if(i > 2) {
						arr.push({
							// 上拉加载更多 加载中... 没有更多了
							loadmore: "上拉加载更多",
							list: []
						})
						continue
					}
					arr.push({
						// 上拉加载更多 加载中... 没有更多了
						loadmore: "上拉加载更多",
						list
					})
				}
				this.newsList = arr
				
			},
			// 关注
			follow(index) {
				this.newsList[this.currentId >> 0].list[index].isFollow = true
				uni.showToast({
					title: "关注成功!"
				})
			},
			// 点赞或踩
			dosupport({dothing, index}) {
				let item = this.newsList[this.currentId >> 0].list[index].support
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
			// 页面切换
			tabbarChange(id) {
				this.currentId = id
				this.scrollInto = `tab${id}`
			},
			// 左右滑动翻页
			swiperChange(e) {
				this.tabbarChange('00'+e.detail.current)
				
			},
			// 滚动到底部事件
			loadmore(index) {
				let item = this.newsList[index]
				// 不处于上拉加载更多就return出去
				if(item.loadmore !== '上拉加载更多') return
				item.loadmore = '加载中...'
				setTimeout(()=>{
					item.list = [...item.list, ...item.list]
					item.loadmore = '上拉加载更多'
				},2000)
			},
			

		},
		// 点击搜索框跳转到搜索页面
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: "../search/search",
				fail: e =>{
					console.log(e);
				}
			})
		},
		// 点击按钮进入文章发布页
		onNavigationBarButtonTap(e) {
			if(e.index === 0) {
				uni.navigateTo({
					url: "/pages/add-input/add-input",
					fail: e =>{
						console.log(e);
					}
				})
			}
		}
		
	}
</script>

<style lang="scss">
	
</style>
