# sl-filter
uni -app filter component

#sl-filter 筛选
筛选组件，组件名：sl-filter

###简介

一款使用简单的筛选组件

###使用方式

在`script`中引用组件

```
 	import slFilter from '@/components/sl-filter/sl-filter.vue';
 	export default {
		components: {
			slFilter
		}
	}
```

###使用示例

```
<template>
	<view class="content">
		<sl-filter :themeColor="themeColor" :menuList="menuList" @result="result"></sl-filter>
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
				filterResult: '',
				menuList: [{
						'title': '职位',
						'detailTitle': '请选择职位类型（可多选）',
						'isMutiple': true,
						'key': 'jobType',
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
						'detailTitle': '请选择月薪范围（可多选）',
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
						'detailTitle': '请选择（单选）',
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
						]
					},
					{
						'title': '排序',
						'key': 'sort',
						'isSort': true,
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
			result(val) {
				console.log('filter_result:' + JSON.stringify(val));
				this.filterResult = JSON.stringify(val, null, 2)
			}
		}
	}
</script>
```

