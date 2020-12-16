<template>
	<view class="content">
		<view >
			<view class="uni-form-item uni-column">
				<input type="tel" class="uni-input" name="" v-model="mobile" placeholder="请输入手机号" />
			</view>
			<!-- <view class="uni-form-item uni-column column-with-btn">
				<input type="text" class="uni-input" name="" placeholder="请输入图片验证码" v-model="captchaImg" />
				<image src="../../static/login-registration/captcha.jpg" mode="" class="img-captcha"></image>
			</view> -->
			<view class="uni-form-item uni-column column-with-btn">
				<input type="number" class="uni-input" name="" placeholder="请输入验证码" v-model="vcode" />
				<button @click="fasong">获取验证码</button>
			</view>
			<view class="uni-form-item uni-column">
				<input type="password" class="uni-input" name="" v-model="password" placeholder="请输入新密码" />
			</view>
			<button type="primary" @click="zhuceHttps">提交</button>
		</view>
		<!-- <view >
			<view class="text-reset">重置成功，新密码已生效！</view>
			<button type="primary" >立即登陆</button>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				control:"User",
				action:"signIn",
				mobile:"" ,  ///手机号码
				vcode:"" ,  ///短信验证码
				password:"" ,  ////密码
				action:"sendSMS"
				}
				
			},
		
		onLoad() {

		},
		methods:{
			zhuceHttps(){
				uni.request({
				    url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
				       method:"POST",
					data: {
				        mobile:this.mobile,
						vcode:this.vcode,
						password:this.password,
						control:"User",
						action:"signIn"
				    },
				    header: {
				        'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
				    },
				    success: (res) => {
				        console.log(res);   
						console.log(this.mobile)
						console.log(this.password)
						if(res.data.status===1){
							uni.switchTab({
								url:'../user/user'
							})
						}
				    },
				});
				console.log("验证码")
			},
			fasong(){
				uni.request({
				    url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
				       method:"POST",
					data: {
				        mobile:this.mobile,
						control:this.control,
						action:this.action	
				    },
				    header: {
				        'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
				    },
				    success: (res) => {
				        console.log(res);   
				    }
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	$color-primary: #b49950;
	.content{
		padding: 100upx;
	}
	.uni-form-item{
		margin-bottom: 42upx;
		padding: 0;
		border-bottom: 1px solid #e3e3e3;
		.uni-input{
			font-size: 40upx;
			padding-left: 7upx;
		}
	}
	.column-with-btn{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		button{
			font-size: 24upx;
			margin: 0;
			width: 180upx;
			text-align: center;
			&:after{
				border: none
			}
			&.active{
				background-color: $color-primary;
				color: $uni-text-color-inverse;
			}
		}
	}
	.img-captcha{
		width: 150upx;
		height: 60upx;
	}
	button[type="primary"]{
		background-color: $color-primary;
		border-radius: 0;
		font-size: 34upx;
		margin-top: 60upx;
	}
	.text-reset{
		text-align: center;
		margin-bottom: 100upx;
		font-size: 36upx;
	}
</style>