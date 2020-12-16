<template>
	<view class="content">
		<view class="list">
			<view class="tishi">若您忘记了密码，可在此重新设置新密码。</view>
			<view class="list-call">
				<image class="img" src="/static/shilu-login/1.png"></image>
				<input class="sl-input" type="number" maxlength="11" placeholder="请输入手机号" v-model="mobile" />
			</view>
			<view class="list-call">
				<image class="img" src="/static/shilu-login/2.png"></image>
				<input class="sl-input" type="text" maxlength="32" placeholder="请输入新密码" v-model="password" />
				<image class="img"></image>
			</view>
			<view class="list-call">
				<image class="img" src="/static/shilu-login/3.png"></image>
				<input class="sl-input" type="number" maxlength="6" placeholder="验证码" v-model="vcode" />
				<wyb-button class="btn" width="190rpx" type="hollow"@click="forgetyanzhen">获取验证码</wyb-button>
				<!-- <view class="yzm" @click="forgetyanzhen">获取验证码1</view> -->
			</view>
		</view>
		<view class="button-login" hover-class="button-hover">
			<text @click="forgetHttps">修改密码</text>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				mobile: '',
				vcode: '',
				password: ''
			}
		},
		onLoad() {

		},
		onUnload() {

		},
		methods: {
			forgetyanzhen(){
				uni.request({
				    url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
				       method:"POST",
					data: {
				        mobile:this.mobile,
						control:'User',
						action:'sendSMS'	
				    },
				    header: {
				        'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
				    },
				    success: (res) => {
				        console.log(res);   
				    }
				});
			},
			
			
			forgetHttps() {
				uni.request({
					url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
					method: "POST",
					data: {
						mobile: this.mobile,
						vcode: this.vcode,
						password: this.password,
						control: "User",
						action: "changePass"
					},
					header: {
						'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
					},
					success: (res) => {
						console.log(res);
						if(res.data.status==1){
							uni.switchTab({
								url:'../user/user'
							})
						}
					},
				});
				console.log("验证码")
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.tishi {
		color: #999999;
		font-size: 28rpx;
		line-height: 50rpx;
		margin-bottom: 50rpx;
	}

	.list {
		display: flex;
		flex-direction: column;
		padding-top: 50rpx;
		padding-left: 70rpx;
		padding-right: 70rpx;
	}

	.list-call {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 100rpx;
		color: #333333;
		border-bottom: 0.5px solid #e2e2e2;
	}

	.list-call .img {
		width: 40rpx;
		height: 40rpx;
	}

	.list-call .sl-input {
		flex: 1;
		text-align: left;
		font-size: 32rpx;
		margin-left: 16rpx;
	}

	.button-login {
		color: #FFFFFF;
		font-size: 34rpx;
		width: 470rpx;
		height: 100rpx;
		background: linear-gradient(-90deg, rgba(63, 205, 235, 1), rgba(188, 226, 158, 1));
		box-shadow: 0rpx 0rpx 13rpx 0rpx rgba(164, 217, 228, 0.2);
		border-radius: 50rpx;
		line-height: 100rpx;
		text-align: center;
		margin-left: auto;
		margin-right: auto;
		margin-top: 100rpx;
	}

	.button-hover {
		background: linear-gradient(-90deg, rgba(63, 205, 235, 0.8), rgba(188, 226, 158, 0.8));
	}

	.yzm {
		color: #FF7D13;
		font-size: 24rpx;
		line-height: 64rpx;
		padding-left: 10rpx;
		padding-right: 10rpx;
		width: auto;
		height: 64rpx;
		border: 1rpx solid rgba(255, 131, 30, 1);
		border-radius: 50rpx;
	}

	.yzms {
		color: #999999 !important;
		border: 1rpx solid #999999;
	}
</style>
