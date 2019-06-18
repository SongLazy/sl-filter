<template>
	
	<view>
		<view style="padding: 0px 0px;">
			<view class="filter-content" v-for="(item, index) in menuList" :key="index" v-if="menuIndex == index">
				<view v-if="item.isSort">
					<view class="filter-content-list">
						<view v-for="(detailItem,idx) in selectDetailList" :key="idx" :class="detailItem.isSelected?'filter-content-list-item-active':'filter-content-list-item-default'"
						 :style="{'color': detailItem.isSelected?themeColor:'#666666'}" @tap="sortTap(idx,selectDetailList,item.key)">
							<text>{{detailItem.title}}</text>
						</view>
					</view>
				</view>
				<view v-else>
					<view class="filter-content-title" v-if="item.detailTitle && item.detailTitle.length">
						<text>{{item.detailTitle}}</text>
					</view>
					<view class="filter-content-detail">
						<text v-for="(detailItem,idx) in selectDetailList" :key="idx" class='filter-content-detail-item-default' :style="{'background-color':detailItem.isSelected?themeColor:'#FFFFFF','color':detailItem.isSelected?'#FFFFFF':'#666666'}"
						 @tap="itemTap(idx,selectDetailList,item.isMutiple,item.key)">
							{{detailItem.title}}
						</text>
					</view>
					<view class="filter-content-footer">
						<view class="filter-content-footer-item" style="color: #777777; background-color: #FFFFFF;" @tap="resetClick(selectDetailList,item.key)">
							<text>重置</text>
						</view>
						<view class="filter-content-footer-item" :style="{'color': '#FFFFFF', 'background-color': themeColor}" @tap="sureClick">
							<text>确定</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				selectArr: [],
				result: {},
				menuIndex: 0,
				selectDetailList: [],
				independenceObj: {},
				selectedKey: '',
				cacheSelectedObj: {}
			};
		},
		props: {
			themeColor: {
				type: String,
				default () {
					return '#D1372C'
				}
			},
			menuList: {
				type: Array,
				default () {
					return []
				}
			},
			independence: {
				type: Boolean,
				default: false
			}
		},
		computed: {
			selectedTitleObj() {
				let obj = {}
				for (let i = 0; i < this.menuList.length; i++) {
					let item = this.menuList[i];
					obj[item.key] = item.title;
				}
				return obj;
			},
			selectedObj: {
				get() {
					let obj = {}
					for (let i = 0; i < this.menuList.length; i++) {
						let item = this.menuList[i];
						if (!this.independence && item.defaultSelectedIndex != null && item.defaultSelectedIndex.toString().length > 0) { // 处理并列菜单默认值

							if (item.isMutiple) {
								obj[item.key] = [];
								item.detailList[0].isSelected = false;
								if (!Array.isArray(item.defaultSelectedIndex)) { // 如果默认值不是数组
									item.defaultSelectedIndex = [item.defaultSelectedIndex];
								}
								for (let j = 0; j < item.defaultSelectedIndex.length; j++) { // 将默认选中的值放入selectedObj
									item.detailList[item.defaultSelectedIndex[j]].isSelected = true;
									obj[item.key].push(item.detailList[item.defaultSelectedIndex[j]].value)
								}

							} else {
								obj[item.key] = item.detailList[item.defaultSelectedIndex].value;
								this.selectedTitleObj[item.key] = item.detailList[item.defaultSelectedIndex].title;
								item.detailList[0].isSelected = false;
								item.detailList[item.defaultSelectedIndex].isSelected = true;
							}
						} else {
							if (item.isMutiple) {
								obj[item.key] = [];
							} else {
								obj[item.key] = '';
							}
						}
					}
					this.result = obj;

					return obj;
				},
				set(newObj) {
					return newObj;
				}

			}
		},
		methods: {
			menuTabClick(index) {

				this.menuIndex = index;
				this.selectDetailList = this.menuList[index].detailList;
				this.selectedKey = this.menuList[index].key;
				// 如果是独立菜单
				if (this.independence && !this.menuList[index].isSort) {
					if (JSON.stringify(this.independenceObj) == '{}') {
						this.initIndependenceObj(index);
					} else {
						for (let key in this.independenceObj) {
							if (key != this.selectedKey) {
								this.initIndependenceObj(index);
								this.resetSelected(this.menuList[index].detailList, this.selectedKey);
							}
						}
					}

				}
				if (this.independence && this.menuList[index].isSort) {

					this.independenceObj = {};


				}
				if (this.independence) {
					let idx = this.menuList[index].defaultSelectedIndex;
					if (idx != null && idx.toString().length > 0) { // 处理独立菜单默认值
						if (this.menuList[index].isMutiple) {
							for (let i = 0; i < idx.length; i++) {
								if (this.menuList[index].detailList[idx[i]].isSelected == false) {
									this.itemTap(idx[i], this.menuList[index].detailList, true, this.selectedKey);
								}

							}
						} else {
							if (this.menuList[index].detailList[idx].isSelected == false) {

								this.itemTap(idx, this.menuList[index].detailList, false, this.selectedKey);

							}
						}

					}
				}


				// #ifdef H5
				this.selectedObj = this.selectedObj;
				this.$forceUpdate();
				// #endif
			},
			initIndependenceObj(index) {
				this.independenceObj = {};
				if (this.menuList[index].isMutiple) {
					this.independenceObj[this.selectedKey] = [];
				} else {
					this.independenceObj[this.selectedKey] = '';
				}
			},
			itemTap(index, list, isMutiple, key) {
				if (isMutiple == true) {
					list[index].isSelected = !list[index].isSelected;
					if (index == 0) {
						this.resetSelected(list, key)
						if (!this.independence) {
							this.selectedTitleObj[key] = list[index].title;
						}
					} else {
						list[0].isSelected = false
						if (list[index].isSelected) {
							if (this.independence) {
								this.independenceObj[this.selectedKey].push(list[index].value);
							} else {
								this.selectedObj[key].push(list[index].value);
							}
						} else {
							list[index].isSelected = false;
							if (this.independence) {
								var idx = this.independenceObj[this.selectedKey].indexOf(list[index].value);
								this.independenceObj[this.selectedKey].splice(idx, 1);
							} else {
								var idx = this.selectedObj[key].indexOf(list[index].value);
								this.selectedObj[key].splice(idx, 1);
							}

						}
						if (this.independence) {
							this.result = this.independenceObj;
						} else {
							this.result = this.selectedObj;
						}

					}
				} else {
					if (index == 0) {
						this.resetSelected(list, key)
						if (!this.independence) {
							this.selectedTitleObj[key] = list[index].title;
						}
					} else {
						list[0].isSelected = false
						if (this.independence) {
							this.independenceObj[this.selectedKey] = list[index].value;
							this.result = this.independenceObj;
						} else {
							this.selectedObj[key] = list[index].value;
							this.result = this.selectedObj;
							this.selectedTitleObj[key] = list[index].title;
						}

						for (let i = 0; i < list.length; i++) {
							if (index == i) {
								list[i].isSelected = true
							} else {
								list[i].isSelected = false
							}
						}
					}
				}
				// #ifdef H5
				this.$forceUpdate();
				// #endif
			},
			resetSelected(list, key) {
				if (typeof this.result[key] == 'object') {
					this.result[key] = [];
				} else {
					this.result[key] = '';
				}
				for (let i = 0; i < list.length; i++) {
					if (i == 0) {
						list[i].isSelected = true;
					} else {
						list[i].isSelected = false;
					}
				}
				// #ifdef H5
				this.$forceUpdate();
				// #endif
			},
			sortTap(index, list, key) {
				if (this.independence) {
					this.independenceObj[this.selectedKey] = list[index].value;
					this.result = this.independenceObj;
				} else {
					this.selectedObj[key] = list[index].value;
					this.result = this.selectedObj;
					this.selectedTitleObj[key] = list[index].title;
				}

				for (let i = 0; i < list.length; i++) {
					if (index == i) {
						list[i].isSelected = true;
					} else {
						list[i].isSelected = false;
					}
				}
				let obj = {
					'result': this.result,
					'titles': this.selectedTitleObj
				}
				this.$emit("confirm", obj);
			},
			sureClick() {
				let obj = {
					'result': this.result,
					'titles': this.selectedTitleObj
				}
				this.$emit("confirm", obj);
			},
			resetClick(list, key) {
				this.resetSelected(list, key)
			}
		}
	}
</script>

<style>
	.filter-content {
		background-color: #F6F7F8;
	}

	.filter-content-title {
		border-bottom: #EEEEEE 1px solid;
		padding: 10px 15px;
		font-size: 13px;
		color: #999999;
	}

	.filter-content-detail {
		padding: 5px 15px;
	}

	.filter-content-detail-item-active {
		background-color: #D1372C;
		color: #FFFFFF;
		padding: 5px 15px;
		border-radius: 20px;
		margin-right: 10px;
		margin-top: 10px;
		display: inline-block;
		font-size: 14px;
	}

	.filter-content-detail-item-default {
		background-color: #FFFFFF;
		color: #666666;
		padding: 5px 15px;
		border-radius: 20px;
		margin-right: 10px;
		margin-top: 10px;
		display: inline-block;
		font-size: 14px;
	}

	.filter-content-footer {
		display: flex;
		justify-content: space-between;
		width: 100%;
		height: 45px;
		margin-top: 10px;
	}

	.filter-content-footer-item {
		width: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 16px;
	}

	.filter-content-list {

		padding: 5px 15px;
	}

	.filter-content-list-item-default {
		color: #666666;
		width: 100%;
		padding: 10px 0px;
	}

	.filter-content-list-item-default text {
		width: 90%;
		font-size: 14px;
		display: inline-block;
	}

	.filter-content-list-item-active {
		color: #D1372C;
		width: 100%;
		padding: 10px 0px;
	}

	.filter-content-list-item-active text {
		font-size: 14px;
		width: 90%;
		display: inline-block;
	}

	.filter-content-list-item-active:after {
		content: '✓';
	}
</style>
