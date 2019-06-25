<template>
	<view class="content">
		<sl-filter :ref="'slFilter'" :topFixed="true" :isTransNav="true" :navHeight="0" :color="titleColor" :themeColor="themeColor" :menuList="menuList"
		 @result="result"></sl-filter>
		<view style="width: 100%;background-color: #0077AA; height: 100px;">
			<!-- 这是一个没有什么用处的占位view，测试组件会不会被其他view挡住 -->
			<text>占位view</text>
		</view>

		<button type="primary" @click="changeMenuList()" style="margin-top: 10px; width: 90%;">动态修改menuList的item</button>
		<button type="primary" @click="changeMenuListDetailList()" style="margin-top: 10px; width: 90%;">动态修改menuList的detailList</button>
		<button type="primary" @click="resetAllSelect()" style="margin-top: 10px; width: 90%;">重置选项(包括默认项)</button>
		<button type="primary" @click="resetSelectToDefault()" style="margin-top: 10px; width: 90%;">重置选项为默认值</button>

		<view class="text">
			<text>{{filterResult}}</text>
		</view>


	</view>
</template>

<script>
	import slFilter from '@/components/sl-filter/sl-filter.vue';
	export default {
		components: {
			slFilter
		},
		data() {
			return {
				themeColor: '#000000',
				titleColor: '#666666',
				filterResult: '',
				menuList: [{
						'title': '职位',
						'detailTitle': '请选择职位类型（可多选）(默认值为[1,2,5])',
						'isMutiple': true,
						'key': 'jobType',
						'defaultSelectedIndex': [1,2,5],
						'detailList': [{
								'title': '不限',
								'value': ''
							},
							{
								'title': 'uni-app',
								'value': 'uni-app'
							},
							{
								'title': 'java开发',
								'value': 'java'
							},
							{
								'title': 'web开发',
								'value': 'web'
							},
							{
								'title': 'Android开发',
								'value': 'Android'
							},
							{
								'title': 'iOS开发',
								'value': 'iOS'
							},
							{
								'title': '测试工程师',
								'value': '测试'
							},
							{
								'title': 'UI设计',
								'value': 'UI'
							},
							{
								'title': 'Ruby开发',
								'value': 'Ruby'
							},
							{
								'title': 'C#开发',
								'value': 'C#'
							},
							{
								'title': 'PHP开发',
								'value': 'php'
							},
							{
								'title': 'Python开发',
								'value': 'Python'
							}
						]

					},
					{
						'title': '月薪',
						'key': 'salary',
						'isMutiple': true,
						'detailList': [{
								'title': '不限',
								'value': ''
							},
							{
								'title': '0~2000',
								'value': '0~2000'
							},
							{
								'title': '2000~3000',
								'value': '2000~3000'
							},
							{
								'title': '3000~4000',
								'value': '3000~4000'
							},
							{
								'title': '4000~5000',
								'value': '4000~5000'
							},
							{
								'title': '5000~6000',
								'value': '5000~6000'
							},
							{
								'title': '6000~7000',
								'value': '6000~7000'
							},
							{
								'title': '7000~8000',
								'value': '7000~8000'
							},
							{
								'title': '8000~9000',
								'value': '8000~9000'
							},
							{
								'title': '9000~10000',
								'value': '9000~10000'
							},
							{
								'title': '10000以上',
								'value': '10000~1000000'
							}
						]

					},
					{
						'title': '单选',
						'key': 'single',
						'isMutiple': false,
						'reflexTitle': true,
						'detailTitle': '请选择（单选）(默认值为1)',
						'defaultSelectedIndex': 1,
						'detailList': [{
								'title': '不限',
								'value': ''
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
							{
								'title': '条件1',
								'value': 'test_1'
							},
							{
								'title': '条件2',
								'value': 'test_2'
							},
							{
								'title': '条件3',
								'value': 'test_3'
							},
							{
								'title': '条件4',
								'value': 'test_4'
							},
							{
								'title': '条件5',
								'value': 'test_5'
							},
							{
								'title': '条件6',
								'value': 'test_6'
							},
							{
								'title': '条件7',
								'value': 'test_7'
							},
							{
								'title': '条件8',
								'value': 'test_8'
							},
						]
					},
					{
						'title': '排序',
						'key': 'sort',
						'isSort': true,
						'reflexTitle': true,
						'defaultSelectedIndex': 2,
						'detailList': [{
								'title': '默认排序',
								'value': ''
							},
							{
								'title': '发布时间',
								'value': 'add_time'
							},
							{
								'title': '薪资最高',
								'value': 'wages_up'
							},
							{
								'title': '离我最近',
								'value': 'location'
							}
						]
					}
				]
			}
		},
		onLoad() {

		},
		methods: {
			changeMenuList() {
				let menuListItem = {
					'title': '职位',
					'detailTitle': '请选择职位类型（单选）(默认值为1)',
					'isMutiple': false,
					'key': 'jobType',
					'defaultSelectedIndex': 1,
					'detailList': [{
							'title': '不限',
							'value': ''
						},
						{
							'title': 'new_1',
							'value': 'new_1'
						},
						{
							'title': 'new_2',
							'value': 'new_2'
						},
						{
							'title': 'new_3',
							'value': 'new_3'
						},
						{
							'title': 'new_4',
							'value': 'new_4'
						},
						{
							'title': 'new_5',
							'value': 'new_5'
						}
					]
				}
				this.menuList[0] = menuListItem;
				this.$refs.slFilter.resetMenuList(this.menuList)
			},
			changeMenuListDetailList() {
				let tempDetailList = [{
						'title': '不限',
						'value': ''
					},
					{
						'title': 'new_1',
						'value': 'new_1'
					},
					{
						'title': 'new_2',
						'value': 'new_2'
					},
					{
						'title': 'new_3',
						'value': 'new_3'
					}
				]
				this.menuList[0].detailList = tempDetailList;
				this.$refs.slFilter.resetMenuList(this.menuList)
			},
			result(val) {
				console.log('filter_result:' + JSON.stringify(val));
				this.filterResult = JSON.stringify(val, null, 2)
			},
			// 重置所有选项，包括默认选项，并更新result
			resetAllSelect() {
				this.$refs.slFilter.resetAllSelect(function(result){
					console.log('重置之后回调的result:'+JSON.stringify(result))
				})
			},
			// 重置选项为设置的默认值，并更新result
			resetSelectToDefault() {
				this.$refs.slFilter.resetSelectToDefault(function(result){
					console.log('重置为默认值之后回调的result:'+JSON.stringify(result))
				})
			}
		}
	}
</script>

<style>
	.text {
		margin-top: 50px;
		margin-left: 20px;
		width: 100%;
	}
</style>
