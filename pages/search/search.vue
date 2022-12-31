<template>
	<view>
		<!-- 搜索历史 -->
		<template v-if="searchList.length === 0">
			<view class="px-1">
				<view class="py-2 font-md">搜索历史</view>
				<view class="flex flex-wrap">
					<view class="border px-1 rounded mx-1 my-1" v-for="(item, index) in historySearchList" :key="index" @click="search(item)">{{item}}</view>
				</view>
			</view>
		</template>
		<template v-else>
			<view v-for="(item,index) in searchList" :key="index">
				<divider></divider>
				<commonList :item="item" :index="index" @follow="follow" @dosupport='dosupport'></commonList>
			</view>
		</template>
	</view>
</template>

<script>
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
	import commonList from "@/component/common/common-list.vue"
	export default {
		components: {
			commonList
		},
		data() {
			return {
				keyword: "",
				searchList: [],
				historySearchList: ['uni-app第二季实战商城类app和小程序', 'vue', 'vue实战', 'uni-app实战', 'vuex实战', 'uni-app第三季实战微信app和小程序开发', 'vuex', '依赖注入']
			}
		},
		methods: {
			search(keyword) {
				console.log(`搜索了：${keyword}`);
				setTimeout(()=>{
					this.searchList= list
				},3000)
			},
			// 关注
			follow(index) {
				this.list[index].isFollow = true
				uni.showToast({
					title: "关注成功!"
				})
			},
			// 点赞或踩
			dosupport({dothing, index}) {
				let item = this.list[index].support
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
		},
		// 输入框内容变化
		onNavigationBarSearchInputChanged(e) {
			this.keyword = e.text
		},
		// 搜索按钮点击事件
		onNavigationBarButtonTap(e) {
			this.search(this.keyword)
			// 隐藏键盘
			uni.hideKeyboard()
		}
	}
</script>

<style>

</style>
