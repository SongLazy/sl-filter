# sl-filter 筛选
筛选组件，组件名：sl-filter

dcloud插件市场地址 [sl-filter](http://ext.dcloud.net.cn/plugin?id=381)

## 简介

一款使用简单的筛选组件，适配app、微信小程序、H5。

>下拉菜单使用了 [popup 弹出层组件](https://ext.dcloud.net.cn/plugin?id=254)

>作者：[1501324336@qq.com](https://ext.dcloud.net.cn/publisher?id=110319)

> 感谢分享

## 效果图

### 并列菜单

![apposition](https://github.com/SongLazy/sl-filter/blob/master/effect/sl-filter1.gif?raw=true '并列菜单')

### 独立菜单

![independence](https://github.com/SongLazy/sl-filter/blob/master/effect/sl-filter2.gif?raw=true '独立菜单')

## 使用方式

在`script`中引用组件

```vue
 	import slFilter from '@/components/sl-filter/sl-filter.vue';
 	export default {
		components: {
			slFilter
		}
	}
```

## 属性说明

### Props

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
menuList | Array |  | 数组元素的个数为菜单item个数
independence | Boolean | false | 是否为独立菜单，不设置该字段默认为并列菜单
titleColor | String | #666666 | menuBar菜单标题颜色
themeColor | String | #000000 | 按钮选中颜色和确认按钮颜色
isTransNav | Boolean | false | 是否需要设置距离顶部的高度。比如你的导航栏为沉浸式导航栏或者自定义导航栏
navHeight | Number | 0 | 单位px。弹出层距离顶部的高度，需先设置:isTransNav="true"
topFixed | Boolean | false | 是否固定在顶部。:topFixed="true"，当页面滑动时，菜单bar固定在顶部。如不需要可不设置该属性。
@result | function |  | 选中条件的回调。参数为回调的条件对象

#### independence

> 默认为false，并列菜单

值 | 说明
---- | ----
true | 独立菜单。筛选菜单每个子菜单选择完毕点击确定回传当前菜单结果
false | 并列菜单：筛选菜单各个子菜单选择完毕点击确定后回传所有结果


## 数据源

### menuList

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
title | String |  | 一级筛选菜单名称
detailTitle | String |  | 子标题，可作为说明。可不设置此字段
isMutiple | Boolean | false | 是否多选。为true时，可选择多个条件；为false时，只能单选。
isSort | Boolean |  | 为true时，为单选排序筛选方式。不填写此字段为默认筛选方式。
key | String |  | 字段名称，作为result返回的条件的key
detailList | Array |  | 条件列表
defaultSelectedIndex | Array/Number |  | 默认选中的选项，可不设置。值为默认选中项的index或index的数组。
reflexTitle | Boolean | false | 是否将选择的结果映射到菜单title上。只有当isMutiple不为true时生效。


##### defaultSelectedIndex 默认选项说明

值类型 | 示例 | 说明
---- | ---- | ----
Array | 'defaultSelectedIndex': [1,2,5] | 当菜单为多选时('isMutiple': true)，如果默认选中多个选项，可将defaultSelectedIndex设置为数组。单选菜单和排序菜单请不要设置数组。
Number | 'defaultSelectedIndex': 1 | 当菜单为单选或多选菜单只有一个默认值时，可设置为Number，如果你不小心设置为了String类型，也是没问题的。


### detailList

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
title | String |  | 条件名称
value | String |  | 条件值

### 数据源格式

```vue
menuList: [
	{
		'title': '菜单1',
		'detailTitle': '子标题1',
		'isMutiple': true,
		'key': 'key_1',
		'detailList': [
			{
				'title': '不限',
				'value': ''
			},
			{
				'title': '条件_1_1',
				'value': 'val_1_1'
			},
			{
				'title': '条件1_2',
				'value': 'val_1_2'
			}
		]
	},
	{
		'title': '菜单2',
		'detailTitle': '子标题2',
		'key': 'key_2',
		'isMutiple': false,
		'detailList': [
			{
				'title': '不限',
				'value': ''
			},
			{
				'title': '条件_2_1',
				'value': 'val_2_1'
			},
			{
				'title': '条件_2_2',
				'value': 'val_2_2'
			}
		]
	},
	{
		'title': '菜单3',
		'detailTitle': '子标题3',
		'key': 'key_3',
		'isSort': true,
		'isMutiple': false,
		'detailList': [
			{
				'title': '条件_3_1',
				'value': 'val_3_1'
			},
			{
				'title': '条件_3_2',
				'value': 'val_3_2'
			},
			{
				'title': '条件_3_3',
				'value': 'val_3_3'
			}
		]
	}
]
```

## 方法
### 动态修改menuList

方法 | 参数 | 说明 | 使用示例
---- | ---- | ---- | ----
resetMenuList(newMenuList) | 新的数据源 | 动态修改数据源的方法。使用此方法，必须给sl-filter设置ref。 | 以:ref="'slFilter'"为例：```this$refs.slFilter.resetMenuList(newMenuList)```

在有些场景下，需要根据列表的数据来确定筛选的条件有哪些，此时你可以在请求完数据后动态修改menuList。

想这样做，你需要给sl-filter设置一个ref。以:ref="'slFilter'"为例：

例子1： 修改menulistItem，可以同时设置数据源属性，比如单选or多选、默认值等等。

```vue
<sl-filter :ref="'slFilter'" :topFixed="true" :isTransNav="true" :navHeight="0" :color="titleColor" :themeColor="themeColor" :menuList="menuList"
		 @result="result"></sl-filter>

<button type="primary" @click="changeMenuList()" style="margin-top: 10px; width: 60%;">动态修改menuList</button>

...
...

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
			}
		]
	}
	this.menuList[0] = menuListItem;
	this.$refs.slFilter.resetMenuList(this.menuList)
}

```

例子2：如果你不需要重新设置数据源属性，你也可以直接修改detailList。

```vue
<sl-filter :ref="'slFilter'" :topFixed="true" :isTransNav="true" :navHeight="0" :color="titleColor" :themeColor="themeColor" :menuList="menuList"
		 @result="result"></sl-filter>
		 
<button type="primary" @click="changeMenuListDetailList()" style="margin-top: 10px; width: 90%;">动态修改menuList的detailList</button>

...
...

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
}

```

### 重置选项


方法 | 参数 | 说明 | 使用示例
---- | ---- | ---- | ----
resetAllSelect(function(result){}) | 重置之后的回调，回调参数```result```为重置之后的结果 | 重置所有选项，包括默认选项。使用此方法，必须给sl-filter设置ref。 | 见下方
resetSelectToDefault(function(result){}) | 重置之后的回调，回调参数```result```为重置之后的结果 | 重置为设置的默认选项。使用此方法，必须给sl-filter设置ref。 | 见下方

有时你需要重置所有菜单的选项，此时每个菜单的重置按钮并不能满足需求，组件提供了两个重置选项的方法，并在方法的参数中通过回调的方式将更新后的结果返回，以便满足不同的业务需求。

使用示例：


```vue
<sl-filter :ref="'slFilter'" :topFixed="true" :isTransNav="true" :navHeight="0" :color="titleColor" :themeColor="themeColor" :menuList="menuList"
		 @result="result"></sl-filter>
		 
<button type="primary" @click="resetAllSelect()" style="margin-top: 10px; width: 90%;">重置选项(包括默认项)</button>
<button type="primary" @click="resetSelectToDefault()" style="margin-top: 10px; width: 90%;">重置选项为默认值</button>

...
...

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

```

## 特别说明

* 请严格按照说明设置数据源
* menuList元素的个数，即为最外层菜单的个数
* 多选条件返回的数据类型是Array，其他为String
* 目前，当数据源不设置`'isSort': true`时，条件的第一项请设置为为“不限”，返回值可以自由设置。当点击了“不限”时，会清空当前条件菜单的条件，与“重置”功能一样。目前不可不设置。

## 使用示例

详细代码见 [github demo](https://github.com/SongLazy/sl-filter)

```vue
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
							}
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
## PS
> **如果能够帮助到你，希望能在github给个星星，谢谢~**


## 更新记录

### 1.1.9

更新日期：2019.06.26

更新内容：增加了重置选项的方法，用于满足需要重置所有菜单选项的场景。分为 重置选项（包括默认值）和 重置为默认值。具体说明请看文档。

### 1.1.8

更新日期：2019.06.18

更新内容：解决选项多了后超出屏幕长度不能滚动的问题

### 1.1.7

更新日期：2019.06.13

更新内容：新增动态修改数据源的方法

方法 | 参数 | 说明 | 使用示例
---- | ---- | ---- | ----
resetMenuList(newMenuList) | 新的数据源 | 动态修改数据源的方法。使用此方法，必须给sl-filter设置ref。 | 以:ref="'slFilter'"为例：```this$refs.slFilter.resetMenuList(newMenuList)```

### 1.1.6

更新日期：2019.06.12

更新内容：

* 当单选和筛选设置默认选项和映射到菜单title时，初始化组件的时候菜单title就会显示默认值

* 修复设置topFixed时在H5上菜单bar会被遮挡的bug

ps: 感谢 *** mzero126@126.com *** 的贡献

### 1.1.5

更新日期：2019.06.10

更新内容：图标本地化。

### 1.1.4

更新日期：2019.06.09

更新内容：当单选菜单设置topFixed为true时，第一项’无限‘也会映射到菜单title。

### 1.1.3

更新日期：2019.06.05

更新内容：新增组件属性：topFixed，是否固定在顶部。

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
topFixed | Boolean | false | 是否固定在顶部。:topFixed="true"，当页面滑动时，菜单bar固定在顶部。如不需要可不设置该属性。

### 1.1.2

更新日期：2019.06.03

更新内容：数据源属性reflexTitle适配H5。

### 1.1.1

更新日期：2019.06.03

更新内容：增加了数据源属性：reflexTitle，是否将选择的结果映射到菜单title上。只有当isMutiple不为true时生效。

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
reflexTitle | Boolean | false | 是否将选择的结果映射到菜单title上。只有当isMutiple不为true时生效。

### 1.1.0

更新日期：2019.05.30

更新内容：增加了组件属性：isTransNav和navHeight，可自定义弹出层距离顶部的高度，用于沉浸式导航栏、自定义导航栏或其他需要的场景。

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
isTransNav | Boolean | false | 是否需要设置距离顶部的高度。比如你的导航栏为沉浸式导航栏或者自定义导航栏。如果不需要，则不用设置此属性
navHeight | Number | 0 | 单位px。弹出层距离顶部的高度，需先设置:isTransNav="true"

### 1.0.9

更新日期：2019.05.23

更新内容：增加menuList数据源中的默认选项设置：defaultSelectedIndex

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
defaultSelectedIndex | Array/Number |  | 默认选中的选项，可不设置。值为默认选中项的index或index的数组。

##### defaultSelectedIndex 默认选项说明

值类型 | 示例 | 说明
---- | ---- | ----
Array | 'defaultSelectedIndex': [1,2,5] | 当菜单为多选时('isMutiple': true)，如果默认选中多个选项，可将defaultSelectedIndex设置为数组。单选菜单和排序菜单请不要设置数组。
Number | 'defaultSelectedIndex': 1 | 当菜单为单选或多选菜单只有一个默认值时，可设置为Number，如果你不小心设置为了String类型，也是没问题的。




### 1.0.8

更新日期：2019.05.21

更新内容：修改了排序key值不正常的bug

### 1.0.7

更新日期：2019.05.21

更新内容：修改了H5端排序(isSort模式)点击后筛选菜单没有渲染的bug

### 1.0.6

更新日期：2019.05.19

更新内容：新增属性：independence 默认值：false--并列菜单

属性名 | 类型 | 默认值 | 说明
---- | ---- | ---- | ----
independence | Boolean | false | 是否为独立菜单，不设置该字段默认为并列菜单

属性说明：

值 | 说明
---- | ----
true | 独立菜单。筛选菜单每个子菜单选择完毕点击确定回传当前菜单结果
false | 并列菜单：筛选菜单各个子菜单选择完毕点击确定后回传所有结果

### 1.0.5

更新日期：2019.05.17

更新内容：适配H5

### 1.0.4

更新日期：2019.05.17

更新内容：适配微信小程序

### 1.0.3

更新日期：2019.05.09

更新内容：pop窗修改为在filterbar下层滑入滑出


### 1.0.2

更新日期：2019.05.08

更新内容：修复了当不设置子标题时，显示空行的bug


### 1.0.1

更新日期：2019.05.08

更新内容：修改了color属性未定义报错的bug

