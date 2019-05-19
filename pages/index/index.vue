<template>
	<view class="content">
		<view class="demo-content">
			<view class="demo-cell">
				<view class="demo-cell-t" @click="toApposition()">
					<text>并列菜单</text>
				</view>
				<view class="demo-cell-b">
					<text>并列菜单：筛选菜单各个子菜单选择完毕点击确定后回传所有结果</text>
				</view>

			</view>
			<view class="demo-cell">
				<view class="demo-cell-t" @click="toIndependence()">
					<text>独立菜单</text>
				</view>
				<view class="demo-cell-b">
					<text>独立菜单：设置插件属性 :independence="true"。筛选菜单每个子菜单选择完毕点击确定回传当前菜单结果</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			toApposition() {
				uni.navigateTo({
					url: '../apposition/index'
				})
			},
			toIndependence() {
				uni.navigateTo({
					url: '../independence/index'
				})
			}
		}
	}
</script>

<style>
	.demo-content {
		margin: 50px 15px;
	}
	.demo-cell {
		padding-top: 50px;
	}
	.demo-cell-t {
		padding: 10px;
		border-radius: 5px;
		border: #EEEEEE 1px solid;
		display: flex;
		justify-content: center;
	}
	.demo-cell-t text {
		font-size: 16px;
		color: #333333;
	}
	.demo-cell-b {
		margin-top: 10px;
	}
	.demo-cell-b text {
		font-size: 13px;
		color: #999999;
	}
</style>
