<template>
	<view v-if="tabs.length>0" class='tab'>
		<view class='row'>
			<view :class="'col-'+gridLeft">
				<view class='row'>
					<block v-for="tab in tabs" :key="tab.value">
						<view @click='tabClick' class="col font-lv2" :class="tab.value == curTab ? 'active ' : ''" :data-title="tab.title"
						 :data-value="tab.value">{{tab.title}}</view>
					</block>
				</view>
			</view>
			<view v-if="gridRight>0 && showSearch" :class="'col-'+gridRight">
				<navigator url='/pages/search/search'>
					<image src='/static/images/search.png'></image>
				</navigator>
			</view>
		</view>
		<slot></slot>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				curTab: '',
				gridRight: 4,
				gridLeft: 8,
			};
		},
		props: {
			tabGridLen: {
				type: Number,
				value: 8, //1 ~ 12
			},
			tabs: {
				type: Array,
				value: []
			},
			activeTab: {
				type: String
			},
			showSearch: {
				type: Boolean,
				value: false,
			}
		},
		created() {
			this.gridLeft = this.tabGridLen == 0 ? 8 : this.tabGridLen
			this.gridRight = 12 - this.gridLeft
			this.curTab = this.activeTab
			if (this.tabs.length > 0 && this.curTab == '') {
				this.curTab = this.tabs[0].value
			}
		},
		watch: {
			curTab: function() {
				this.curTab = this.activeTab
			}
		},
		methods: {
			tabClick(e) {
				this.curTab = e.target.dataset.value
				this.$emit('tabClick', e.target.dataset)
			}
		}
	}
</script>

<style>
	.tab {
		border-bottom: 1upx solid #ddd;
		box-sizing: border-box;
		padding: 0 15px;
		height: 40px;
		line-height: 40px;
	}

	.tab .col {
		text-align: center;
		height: 40px;
	}

	.tab .active {
		color: #00acff;
		border: 1upx solid #ddd;
		border-bottom: 0;
		background-color: #fff;
		border-top-left-radius: 6upx;
		border-top-right-radius: 6upx;
		box-sizing: border-box;
	}

	.tab navigator {
		float: right;
		padding: 0 8px 0 15px;
	}

	.tab image {
		width: 25px;
		height: 25px;
		top: 6.5px;
		position: relative;
	}

	@media (min-width: 768px) {
		.tab {
			height: 50px;
			line-height: 50px;
		}

		.tab .col {
			height: 50px;
		}

		.tab image {
			width: 30px;
			height: 30px;
			top: 5upx;
		}
	}
</style>
