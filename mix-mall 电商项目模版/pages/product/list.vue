<template>
	<view class="content">
		 <view>
		        <liuyuno-tabs :tabData="list" :defaultIndex="defaultIndex" @tabClick='tabClick' />
		    </view>
		 <load-refresh
		      ref="loadRefresh"
		      :pageNo="currPage"
		      :totalPageNo="totalPage" 
		      @loadMore="loadMore" 
		      @refresh="refresh">
		      <view slot="content-list">
		        <!-- 数据列表 -->
		       <view class="goods-list">
		       	<view 
		       		v-for="(item, index) in goodsList" :key="index"
		       		class="goods-item"
		       		@click="navToDetailPage(item.id)"
		       	>
		       		<view class="image-wrapper" >
		       			<image :src="item.pic" mode="aspectFill"></image>
		       		</view>
		       		<text class="title clamp">{{item.title}}</text>
		       		<view class="price-box">
		       			<text class="price">{{item.coin}}</text>
		       		
		       		</view>
		       	</view>
		       </view>
		      </view>
		    </load-refresh>
		  
		    
	</view>
</template>

<script>
      import loadRefresh from '@/components/load-refresh/load-refresh.vue';
	   import liuyunoTabs from "@/components/liuyuno-tabs/liuyuno-tabs.vue";
	export default {
		
		 components: {
		    liuyunoTabs,loadRefresh
		  },
		data() {
			return {
				fenleiList:[],
				change:0,
				list:[],
				defaultIndex:'',
				goodsList:[],
				aa:[],
				currPage: 1, // 当前页码
				        totalPage: 1 // 总页数
			}
		},
		onLoad(){
			this.fenlei()
			this.tabClick()
			this.shuaxin()
		},
		methods: {
			
			
			 // 上划加载更多
			      loadMore() {
			        console.log('loadMore')
			        // 请求新数据完成后调用 组件内loadOver()方法
			        // 注意更新当前页码 currPage
			        this.$refs.loadRefresh.loadOver()
			      },
			      // 下拉刷新数据列表
			      refresh() {
			        console.log('refresh')
			      },
			
			
			
		//页面刷新
			shuaxin(){
				uni.request({
					url: 'https://yohigame.cnyouwei.com/app.php',
						method: "POST",
						data: {
							  control:"Shop",
							   action:"getProduct",
							    type:2,
								 cateId:0,
								  priceSort:0,
								  lastId:0
						},
						header: {
							'Content-Type': 'application/x-www-form-urlencoded', //自定义请求头信息
						},
						success: (res) => {
							
							this.goodsList=res.data.result
						       this.goodsList.forEach((item,index)=>{
								 console.log(item.pic);
								 item.pic= "https://yohigame.cnyouwei.com"+item.pic
				                 
							   })
							console.log(this.goodsList)
						}
				})
			},
			fenlei(){
				uni.request({
					url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
					method: "POST",
					data: {
						  control:"Shop",
						   action:"getCate"
					},
				
					header: {
						'Content-Type': 'application/x-www-form-urlencoded', //自定义请求头信息
					},
					success: (res) => {
				            var list=[];
					
						this.fenleiList=res.data.result
						this.list.push({name:"全部"})
						this.fenleiList.forEach((item,index)=>{
							console.log(item)
							this.list.push(item)
						})
						console.log(this.list)	
				}	
				});
			},
			tabClick(defaultIndex){
			 this.change=defaultIndex
			console.log(defaultIndex)
				if(defaultIndex==0){
					uni.request({
						url: 'https://yohigame.cnyouwei.com/app.php',
							method: "POST",
							data: {
								  control:"Shop",
								   action:"getProduct",
								    type:0,
									 cateId:0,
									  priceSort:0,
									  lastId:0
							},
							header: {
								'Content-Type': 'application/x-www-form-urlencoded', //自定义请求头信息
							},
							success: (res) => {
								
								this.goodsList=res.data.result
							       this.goodsList.forEach((item,index)=>{
									 console.log(item.pic);
									 item.pic= "https://yohigame.cnyouwei.com"+item.pic
					                 
								   })
								console.log(this.goodsList)
							}
					})
				}
			else if(defaultIndex==1){
				console.log(123)
			}
			},
			navToDetailPage(id){
				console.log(id)
				uni.navigateTo({
					url:"/pages/product/product?uid="+id
				})
			}
		}
	}
</script>

<style lang="scss">
	
	.goods-list{
		display:flex;
		flex-wrap:wrap;
		padding: 0 30upx;
		background: #fff;
		.goods-item{
			display:flex;
			flex-direction: column;
			width: 48%;
			padding-bottom: 40upx;
			&:nth-child(2n+1){
				margin-right: 4%;
			}
		}
		.image-wrapper{
			width: 100%;
			height: 330upx;
			border-radius: 3px;
			overflow: hidden;
			image{
				width: 100%;
				height: 100%;
				opacity: 1;
			}
		}
		.title{
			font-size: $font-lg;
			color: $font-color-dark;
			line-height: 80upx;
		}
		.price-box{
			display: flex;
			align-items: center;
			justify-content: space-between;
			padding-right: 10upx;
			font-size: 24upx;
			color: $font-color-light;
		}
		.price{
			font-size: $font-lg;
			color: $uni-color-primary;
			line-height: 1;
			&:before{
				content: '游戏金币';
				font-size: 26upx;
			}
		}
	}
	
	.nav{
		width: 100%;
		height: 100upx;
	     display: flex;
		overflow: auto;
	}
 .nav_a{
	      width: 200upx;
		  height: 50upx;
		background: #4399FC;
	}
	.active{
		color:#909399;
	}
	
</style>
