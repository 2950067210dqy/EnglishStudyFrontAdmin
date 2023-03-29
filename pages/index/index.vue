<template>
	<view class="main" v-if="user!=null">
		
		<view class="header"  style="background: linear-gradient(90deg, #0d0d0d, #595959);padding: 20rpx;width: 100%;border-bottom: 1rpx grey solid;text-align: right;justify-content: right;position: relative;">	
			<view style="display: inline-block;;color: white;position: absolute;left: 2%;top: 30%;font-size: 39rpx;">
				{{globalData.getDateTime(3)}}
			</view>
			<view style="display: inline-block;vertical-align: middle;color: aliceblue;">
				{{user!=null?user.type==-1?'超级管理员':'管理员':''}}:<text style="font-weight: bolder;margin-left: 20rpx;margin-right: 20rpx;font-size: 39rpx;border-bottom: 1rpx solid whitesmoke;">{{user.name}}</text>
			</view>
			<view style="display: inline-block;vertical-align: middle;" @click="showActionBarByMain">
				<image style="margin-right: 50rpx;width: 100rpx;height: 100rpx;border-radius: 50%;" @error="imageError" :src="user.headimage"></image>
			</view>
			
			
		</view>
		<view class="main" style="width: 100%;display: flex;flex-direction: row;flex-wrap: nowrap;overflow: hidden;vertical-align: middle;">
			<view class="left" style="width: 16%;background: black;color: white;" >
				<view class="menu" style="display: flex;flex-direction: column;flex-wrap: wrap;text-align: center;justify-content: center;">
					<scroll-view :scroll-y="true" style="height: 90vh;" :scroll-into-view="'menu'+menu">
							<view id="menu0" @click="clickMenu(0)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==0?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="fire-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;基本信息
							</view>
							<view id="menu1" @click="clickMenu(1)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==1?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="gear" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;首页设置
							</view>
							<view id="menu2" @click="clickMenu(2)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==2?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="staff-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;用户管理
							</view>	
							<view id="menu3" @click="clickMenu(3)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==3?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="wallet-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;词库管理
							</view>
							<view id="menu4"  @click="clickMenu(4)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==4?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="font" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;单词管理
							</view>
							<view id="menu5" @click="clickMenu(5)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==5?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="chatboxes-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;阅读管理
							</view>
							<view id="menu6" @click="clickMenu(6)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==6?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="compose" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;题库管理
							</view>
							<view id="menu7" @click="clickMenu(7)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==7?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="smallcircle" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;积分管理
							</view>
							<view id="menu8" @click="clickMenu(8)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==8?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="gift-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;商品管理
							</view>
							<view id="menu9" @click="clickMenu(9)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==9?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="calendar-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;订单管理
							</view>
							<view id="menu10" @click="clickMenu(10)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==10?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="medal-filled" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;数据管理
							</view>
							<view id="menu11" @click="clickMenu(11)" class="menuitem" :style="`padding:25rpx;padding-left:0rpx;background:${menu==11?'rgb(126, 126, 126)':''};width: 100%;font-size: 35rpx;margin-top: 60rpx;vertical-align: middle;`">
								<uni-icons type="pyq" color="rgb(206, 206, 206)" size="20"></uni-icons>&nbsp;学习圈管理
							</view>
							<view style="height:100rpx;">
								
							</view>
					</scroll-view>
					
				</view>
			</view>
			<view class="right" style="width: 84%;">
				<scroll-view :scroll-y="true" style="height:100vh;">
				<view class="body">
					
					<view v-if="menu==0" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;">
						<!-- 基本信息view start -->
						<view style="display: flex;flex-direction: row;flex-wrap: wrap;padding: 20rpx;justify-content: center;">
							<view class="baseColum" style="height: 42vh;">
								<!-- 用户管理start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`用户管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(2)">
										<uni-icons type="staff-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="userBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日注册：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{userBasicSetting.todayCount}}</text>位.
									</view>
									<view style="margin-top: 10rpx;">
										用户总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{userBasicSetting.allCount}}</text>位.
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近注册：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height: 310rpx;">
												<template v-for="(item,index) in userBasicSetting.lastRegisters" :key="index">
													<view style="margin-top: 7rpx;">
														<view style="display: inline-block;">
															<image @error="basicUserSettingImageError(index)" style="width: 40rpx;height: 40rpx;" :src="item.headimage"></image>
														</view>
														<view class="titles1" style="width: 85%;font-weight: bolder;;margin-left: 6rpx;font-size: 18rpx;">
															{{item.name}}
														</view>
														<view style="display: inline-block;margin-left:40rpx;font-size: 18rpx;">
															{{globalData.getDateDiff(item.createtime) }}
														</view>
													</view>
												</template>
											</scroll-view>
										</view>
									</view>
								</view>
								<!-- 用户管理end -->
							</view>	
							<view class="baseColum" style="height: 42vh;width: 63%;">
								<!-- 单词管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`单词管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(4)">
										<uni-icons type="font" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="wordBasicSetting!=null">
									<view style="margin-top: 2rpx;">
										单词总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{wordBasicSetting.allCount}}</text>个.
									</view>
									<view style="margin-top: 10rpx;" >
										
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;overflow: hidden;">
											<scroll-view :scroll-x="true"  style="height: 420rpx;width: 100%;">
												
													<qiun-data-charts
													     type="line"
														 :opts="wordGraphOpts"
													     :chartData="wordBasicSetting.data"
													   />
											
												
											</scroll-view>	
										</view>
									</view>
								</view>
								<!-- 单词管理 end -->
							</view>
							<view class="baseColum" style="width: 63%;height: 55vh;">
								<!-- 积分管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`积分管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(7)">
										<uni-icons type="smallcircle" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content"  v-if="scoreBasicSetting!=null">
									<view style="width:100%;">
										最高积分：<text style="margin-right: 10rpx;font-weight: bolder;color: red;vertical-align: middle">{{scoreBasicSetting.max.num}}</text>分.
										<view style="display: inline-block;vertical-align: middle;width: 300rpx;">
											<view style="display: inline-block;vertical-align: middle">
												<image @error="basicScoreSettingImageError" style="width: 40rpx;height: 40rpx;" :src="scoreBasicSetting.max.user.headimage"></image>
											</view>
											<view class="titles1" style="width:75%;display: inline-block;font-weight: bolder;;margin-left: 12rpx;font-size: 18rpx;vertical-align: middle">
												{{scoreBasicSetting.max.user.name}}
											</view>
											
										</view>
										<view style="display: inline-block;vertical-align: middle;margin-left: 50rpx;">
											总流水：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{scoreBasicSetting.allMoney}}</text>元.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;vertical-align: middle">
											今日流水：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{scoreBasicSetting.todayMoney}}</text>元.
										</view>			
									</view>
									<view style="margin-top: 10rpx;">
																	
									</view>
								
									<view style="margin-top: 10rpx;overflow: hidden;">
										<swiper :indicator-dots="true" indicator-active-color="rgb(118, 118, 118)" style="height:750rpx;" indicator-color="white" :vertical="true" >
											<swiper-item  >
												<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="31rpx" :title="`日流水`"></uni-section>
												<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;">
													<scroll-view :scroll-x="true" :scroll-y="true" style="height: 520rpx;width: 100%;">
														<view style="width: 1500rpx;" v-if="scoreBasicSetting.data[0].categories!=null">
															<qiun-data-charts
															     type="line"
																 :opts="scoreGraphOpts"
															     :chartData="scoreBasicSetting.data[0]"
															   />
														</view>
														<view v-else style="padding: 20rpx;">
															暂无数据
														</view>
													</scroll-view>	
												</view>
											</swiper-item>
											<swiper-item  >
												<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="31rpx" :title="`周流水`"></uni-section>
												<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;overflow: hidden;">
													<scroll-view :scroll-x="true" :scroll-y="true" style="height:520rpx;width: 100%;">
														<view style="width: 1500rpx;" v-if="scoreBasicSetting.data[1].categories!=null">
															<qiun-data-charts
															     type="line"
																:opts="scoreGraphOpts"
															     :chartData="scoreBasicSetting.data[1]"
															   />
														</view>
														<view v-else style="padding: 20rpx;">
															暂无数据
														</view>
													</scroll-view>	
												</view>
											</swiper-item>
											<swiper-item  >
												<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="31rpx" :title="`月流水`"></uni-section>
												<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;overflow: hidden;">
													<scroll-view :scroll-x="true" :scroll-y="true" style="height:520rpx;width: 100%;">
														<view style="width: 1500rpx;" v-if="scoreBasicSetting.data[2].categories!=null">
															<qiun-data-charts
															     type="line"
																 :opts="scoreGraphOpts"
															     :chartData="scoreBasicSetting.data[2]"
															   />
														</view>
														<view v-else style="padding: 20rpx;">
															暂无数据
														</view>
													</scroll-view>	
												</view>
											</swiper-item>
										</swiper>		
									</view>
								</view>
								<!-- 积分管理 end -->
							</view>
							<view class="baseColum" style="height: 55vh;">
								<!-- 订单管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`订单管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(9)">
										<uni-icons type="calendar-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="orderBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										<view style="display: inline-block;">
											今日下单：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{orderBasicSetting.todayCount}}</text>个.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;">
											订单总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{orderBasicSetting.allCount}}</text>个.
										</view>
										
									</view>
									<view style="margin-top: 10rpx;" >
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="22rpx" :title="`订单总数构成`"></uni-section>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #000000);padding: 5rpx;overflow: hidden;">
											<scroll-view  style="height: 530rpx;width: 100%;">
												<view style="height: 530rpx">
													<qiun-data-charts
													     type="pie"
															:opts="basciOrderOpts"
													     :chartData="orderBasicSetting.data"
													   />
												</view>
												
											</scroll-view>	
										</view>
									</view>
								</view>
								<!-- 订单管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 阅读管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`阅读管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(5)">
										<uni-icons type="chatboxes-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="readBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日创建：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{readBasicSetting.todayCount}}</text>篇.
									</view>
									<view style="margin-top: 10rpx;">
										文章总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{readBasicSetting.allCount}}</text>篇.
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近创建：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #3b3b3b);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height: 490rpx;">
												<template v-for="(item,index) in readBasicSetting.nodes" :key="index">
													<view style="margin-top: 10rpx;border-bottom: 1rpx solid grey;">
														<view style="display: inline-block;font-weight: bolder;">
															《{{item.dsc}}》
														</view>
														<view class="titles1" style="width: 100%;;margin-left: 6rpx;font-size: 18rpx;margin-top: 10rpx;">
															文章量：{{item.count}} 篇.
														</view>
														<view style="width: 100%;;margin-left: 6rpx;font-size: 18rpx;margin-top: 10rpx;">
															所属大类: {{item.parentDsc}}.
														</view>
														
														<view style="margin-bottom: 20rpx;display: inline-block;margin-left: 6rpx;font-size: 18rpx;">
															{{globalData.getDateDiff(item.createtime) }}
														</view>
													</view>
												</template>
											</scroll-view>	
										</view>
									</view>
								</view>
								<!-- 阅读管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 题库管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`题库管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(6)">
										<uni-icons type="compose" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="testBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日创建：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{testBasicSetting.todayCount}}</text>个.
									</view>
									<view style="margin-top: 10rpx;">
										题库总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{testBasicSetting.allCount}}</text>个.
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近创建：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #505050);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height:  490rpx;">
												<template v-for="(item,index) in testBasicSetting.nodes" :key="index">
													<view style="margin-top: 10rpx;border-bottom: 1rpx solid grey;">
														<view style="display: inline-block;font-weight: bolder;">
															《{{item.dsc}}》
														</view>
														<view class="titles1" style="width: 100%;;margin-left: 6rpx;font-size: 18rpx;margin-top: 10rpx;">
															题目量：{{item.count}} 道.
														</view>
							
														<view style="margin-bottom: 20rpx;display: inline-block;margin-left: 6rpx;font-size: 18rpx;">
															{{globalData.getDateDiff(item.createtime) }}
														</view>
													</view>
												</template>
											</scroll-view>	
										</view>
									</view>
								</view>
								<!-- 题库管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 词库管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`词库管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(3)">
										<uni-icons type="wallet-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="cikuBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日创建：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{cikuBasicSetting.todayCount}}</text>个.
									</view>
									<view style="margin-top: 10rpx;">
										词库总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{cikuBasicSetting.allCount}}</text>个.
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近创建：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #505050);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height:  490rpx;">
												<template v-for="(item,index) in cikuBasicSetting.nodes" :key="index">
													<view style="margin-top: 10rpx;border-bottom: 1rpx solid grey;">
														<view style="display: inline-block;font-weight: bolder;">
															《{{item.dsc}}》
														</view>
														<view class="titles1" style="width: 100%;;margin-left: 6rpx;font-size: 18rpx;margin-top: 10rpx;">
															单词量：{{item.count}} 个.
														</view>
														<view style="width: 100%;;margin-left: 6rpx;font-size: 18rpx;margin-top: 10rpx;">
															所属大类: {{item.parentDsc}}.
														</view>
														
														<view style="margin-bottom: 20rpx;display: inline-block;margin-left: 6rpx;font-size: 18rpx;">
															{{globalData.getDateDiff(item.createtime) }}
														</view>
													</view>
												</template>
											</scroll-view>	
										</view>
									</view>
								</view>
								<!-- 词库管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 学习圈管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`学习圈管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(11)">
										<uni-icons type="pyq" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="studyCircleBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日发布：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{studyCircleBasicSetting.todayCount}}</text>篇.
									</view>
									<view style="margin-top: 10rpx;">
										学习圈总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{studyCircleBasicSetting.allCount}}</text>篇.
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近创建：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #505050);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height: 480rpx;">
												<template v-for="(item,index) in studyCircleBasicSetting.lastStudyCircles" :key="index">
													<view style="margin-top: 7rpx;">
														<view style="display: inline-block;">
															<image @error="basicStudyCircleSettingImageError(index)" style="width: 40rpx;height: 40rpx;" :src="item.user.headimage"></image>
														</view>
														<view class="titles1" style="width: 85%;font-weight: bolder;;margin-left: 6rpx;font-size: 18rpx;">
															{{item.user.name}}
														</view>
														<view class="titles1" style="font-weight: bolder;width: 35%;;display: inline-block;margin-left:40rpx;font-size: 25rpx;font-style: italic;">
															{{item.title}}
														</view>
														<view class="titles1" style="width: 50%;display: inline-block;margin-left:40rpx;font-size: 18rpx;">
															{{globalData.getDateDiff(item.updatetime) }}
														</view>
													</view>
												</template>
											</scroll-view>
										</view>
									</view>
								</view>
								<!-- 学习圈管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 数据管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`数据管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(10)">
										<uni-icons type="medal-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="dataBasicSetting!=null">
									
									<view style="margin-top: 150rpx;">
										<view style="display: inline-block;">
											今日学习单词：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.todayStudyCount}}</text>个.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;">
											学习单词总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.allStudyCount}}</text>个.
										</view>
										
									</view>
									<view style="margin-top: 50rpx;">
										<view style="display: inline-block;">
											今日复习单词：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.todayReviewCount}}</text>个.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;">
											复习单词总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.allReviewCount}}</text>个.
										</view>
										
									</view>
									<view style="margin-top: 50rpx;font-size: 27rpx;">
										<view style="display: inline-block;">
											今日学习时间：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.todayTimeCount}}</text>分钟.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;">
											学习时间总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.allTimeCount}}</text>分钟.
										</view>
										
									</view>
									<view style="margin-top: 50rpx;">
										<view style="display: inline-block;">
											今日学习用户：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.todayUserCount}}</text>位.
										</view>
										<view style="display: inline-block;margin-left: 20rpx;">
											学习用户总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{dataBasicSetting.allUserCount}}</text>位.
										</view>
										
									</view>
								</view>
								<!-- 数据管理 end -->
							</view>
							<view class="baseColum" style="height: 53vh;">
								<!-- 商品管理 start -->
								<view class="title" style="width: 100%;border-bottom: 1rpx solid grey;">
									<view style="display: inline-block;width: 85%;">
										<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`商品管理`"></uni-section>
									</view>
									<view style="display: inline-block;cursor: pointer;" @click="clickMenu(8)">
										<uni-icons type="gift-filled" color="white" size="20"></uni-icons>
									</view>
								</view>
								<view class="content" v-if="productBasicSetting!=null">
									<view style="margin-top: 10rpx;">
										今日上架：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{productBasicSetting.todayCount}}</text>件.
									</view>
									<view style="margin-top: 10rpx;">
										商品总数：<text style="margin-right: 10rpx;font-weight: bolder;color: red;">{{productBasicSetting.allCount}}</text>件
									</view>
									<view style="margin-top: 10rpx;">
										<view style="margin-bottom: 2rpx;">
											最近上架：
										</view>
										<view style="border-radius: 20rpx;background: linear-gradient(90deg, #0d0d0d, #505050);padding: 5rpx;">
											<scroll-view :scroll-y="true" style="height: 490rpx;">
												<uni-row>
												<template v-for="(item, index) in productBasicSetting.nodes" :key="index">
													<uni-col :span="12">
														<uni-card :cover="item.products.imgAddre.indexOf('//')==-1?(globalData.url+'static/image/productImage/'+item.products.imgAddre):item.products.imgAddre" @imageError="basicProductSettingImageError(index,0)">
															<!-- <image slot='cover' style="width: 100%;" :src="cover"></image> -->
												
															<text class="uni-body titles2" >{{ item.products.title }}</text>
															<view style="vertical-align:middle;color: rgb(159, 159, 159);">库存:{{ item.products.num }}</view>
															<view style="vertical-align:middle;color: rgb(159, 159, 159);font-size: 16rpx;">{{ item.parentDsc }}-{{item.dsc}}</view>
															<view style="vertical-align:middle;font-size: 18rpx;color: grey;font-size: 16rpx;">
																{{globalData.getDateDiff(item.products.time) }}
															</view>
															<view style="vertical-align:middle;margin-top: 7rpx;" v-if="item.merchant!=null">
																<view style="display: inline-block;">
																	
																	<image @error="basicProductSettingImageError(index,1)" style="width: 40rpx;height: 40rpx;" v-if="item.merchant.headimage!=null" :src="item.merchant.headimage"></image>
																</view>
																<view class="titles1" style="width: 65%;font-weight: bolder;;margin-left: 6rpx;font-size: 18rpx;">
																	{{item.merchant.name}}
																</view>
																
															</view>
															<view class="price" style="margin-top: 20rpx;vertical-align:middle;">
																<view style="display: inline-block;vertical-align:middle;"><uni-icons type="smallcircle" color="#daa520" size="20"></uni-icons></view>
																<view style="display: inline-block;vertical-align:middle;color:#daa520;font-size: 30rpx;font-weight: bolder;">
																	<view style="">{{ item.products.price }}</view>
																</view>
															</view>
												
															
														</uni-card>
													</uni-col>
												</template>
												</uni-row>
											</scroll-view>
									
										</view>
									</view>
								</view>
								<!-- 商品管理 end -->
							</view>
						</view>
						<!-- 基本信息view end -->
					</view>
					
					
					
					<view v-if="menu==1" style="display: flex;flex-direction: row;flex-wrap: nowrap;position: relative;background: linear-gradient(90deg, #0d0d0d, #595959);padding: 20rpx;" >
						<!-- 首页设置view start -->
						<view style="width: 65%;padding-top: 30rpx;position: relative;"  >
							<scroll-view :scroll-y="true" style="height: 87vh;width: 64%;">
								<uni-forms label-width="1500" ref="indexSetting" :rules="rules" :modelValue="indexSetting" label-position="top" >
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[everyDayWord] '+indexSetting.data.everyDayWordModule.everyDayWord.desc" labelColor="rgb(236, 236, 236)"   name="everyDayWord">
										<uni-easyinput 
										@blur="binddata('everyDayWord', indexSetting.data.everyDayWordModule.everyDayWord.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.everyDayWordModule.everyDayWord.value"
											:placeholder="'请输入'+indexSetting.data.everyDayWordModule.everyDayWord.desc"
											
											
										>
											<template v-slot:code>
												<view style="text-align: center;">
													<button  @tap="randomEveryDayWord" type="default" style="display: inline-block;text-align: center;font-size: 20rpx;">
														<text style="text-align: center;">随机</text>
													</button>
												</view>
											</template>
										</uni-easyinput>
									</uni-forms-item>
									<uni-forms-item  v-if="indexSetting!=null" :required="true" :label="'[baseEveryDayWordSize] '+indexSetting.data.everyDayWordModule.baseEveryDayWordSize.desc" labelColor="rgb(236, 236, 236)"   name="baseEveryDayWordSize">
										<uni-easyinput 
										@blur="binddata('baseEveryDayWordSize', indexSetting.data.everyDayWordModule.baseEveryDayWordSize.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="number"
											v-model="indexSetting.data.everyDayWordModule.baseEveryDayWordSize.value"
											:placeholder="'请输入'+indexSetting.data.everyDayWordModule.baseEveryDayWordSize.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[baseEveryDayWordFactor] '+indexSetting.data.everyDayWordModule.baseEveryDayWordFactor.desc" labelColor="rgb(236, 236, 236)"   name="baseEveryDayWordFactor">
										<uni-easyinput 
										@blur="binddata('baseEveryDayWordFactor', indexSetting.data.everyDayWordModule.baseEveryDayWordFactor.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="number"
											v-model="indexSetting.data.everyDayWordModule.baseEveryDayWordFactor.value"
											:placeholder="'请输入'+indexSetting.data.everyDayWordModule.baseEveryDayWordFactor.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[baseEveryDayWordMutipiNum] '+indexSetting.data.everyDayWordModule.baseEveryDayWordMutipiNum.desc" labelColor="rgb(236, 236, 236)"   name="baseEveryDayWordMutipiNum">
										<uni-easyinput 
										@blur="binddata('baseEveryDayWordMutipiNum', indexSetting.data.everyDayWordModule.baseEveryDayWordMutipiNum.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="number"
											v-model="indexSetting.data.everyDayWordModule.baseEveryDayWordMutipiNum.value"
											:placeholder="'请输入'+indexSetting.data.everyDayWordModule.baseEveryDayWordMutipiNum.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[signInText] '+indexSetting.data.signInModule.signInText.desc" labelColor="rgb(236, 236, 236)"   name="signInText0">
										<uni-easyinput 
										@blur="binddata('signInText', indexSetting.data.signInModule.signInText.value[0],'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.signInModule.signInText.value[0]"
											:placeholder="'请输入'+indexSetting.data.signInModule.signInText.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[signInText] '+indexSetting.data.signInModule.signInText.desc" labelColor="rgb(236, 236, 236)"   name="signInText1">
										<uni-easyinput 
										@blur="binddata('signInText', indexSetting.data.signInModule.signInText.value[1],'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.signInModule.signInText.value[1]"
											:placeholder="'请输入'+indexSetting.data.signInModule.signInText.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[learnText] '+indexSetting.data.learnModule.learnText.desc" labelColor="rgb(236, 236, 236)"   name="learnText">
										<uni-easyinput 
										@blur="binddata('learnText', indexSetting.data.learnModule.learnText.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.learnModule.learnText.value"
											:placeholder="'请输入'+indexSetting.data.learnModule.learnText.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[wordBankText] '+indexSetting.data.wordBankModule.wordBankText.desc" labelColor="rgb(236, 236, 236)"   name="wordBankText">
										<uni-easyinput 
										@blur="binddata('wordBankText', indexSetting.data.wordBankModule.wordBankText.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.wordBankModule.wordBankText.value"
											:placeholder="'请输入'+indexSetting.data.wordBankModule.wordBankText.desc"
										/>
									</uni-forms-item>
									<uni-forms-item v-if="indexSetting!=null" :required="true" :label="'[reviewText] '+indexSetting.data.reviewModule.reviewText.desc" labelColor="rgb(236, 236, 236)"   name="reviewText">
										<uni-easyinput 
										@blur="binddata('reviewText', indexSetting.data.reviewModule.reviewText.value,'indexSetting')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="indexSetting.data.reviewModule.reviewText.value"
											:placeholder="'请输入'+indexSetting.data.reviewModule.reviewText.desc"
										/>
									</uni-forms-item>
								</uni-forms>
								<view style="height: 20rpx;"></view>
							</scroll-view>
							
							<view style="position: absolute;top: 20rpx;right:10%;background: black;color: white;padding: 50rpx;border-radius: 20rpx;">
								<view>
									<button @click="confirmUpdateSetting"  type="primary" size="mini" style="font-size: 30rpx;">确定修改</button>
								</view>
								<view style="margin-top: 50rpx;">
									<button @click="cleanDefaultSetting" type="warn" size="mini" style="font-size: 30rpx;">清除修改</button>
								</view>
								<view style="margin-top: 50rpx;">
									<button @click="toOriginSetting" type="default" size="mini" style="font-size: 30rpx;">恢复初始</button>
								</view>
							</view>
							<view style="position: absolute;top: 500rpx;right:1%;background: black;color: white;padding:20rpx;border-radius: 20rpx;">
								<view>
									<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`修改背景`"></uni-section>
								</view>
								<view>
									<uni-file-picker
										@select="backgroundSelect"
										@delete="backgroundDelete"
										ref="importheadimage"
										limit="1"
										fileMediatype="image"
										title="最多选择1张图片(png,jpg,gif)"
										titleColor="white"
										titleFontSize="35rpx"
										fileExtname="png,jpg,gif"
										
									></uni-file-picker>
								</view>
								
								
							</view>
						</view>
						<view style="position: absolute;top: 0;right: 0;">
							<web-view style="width: 900rpx;height: 90vh" :fullscreen="false"  :src="globalData.webUrl"></web-view>
						</view>
						<!-- 首页设置view end -->
					</view>
					<view v-if="menu==2"  style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;">
						<!-- 用户管理view start -->
						<view style="padding: 20rpx;">
							<scroll-view class="scrollXTarBar" :scroll-x="true">
								<template v-for="(item, index) in userModuleMenuTexts" :key="index">
									<view @tap="onClickUserModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', userModuleMenu == index ? 'activeText' : '']">{{ item }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
							</scroll-view>
						</view>
						<view style="justify-content: right;text-align: right;padding: 20rpx;vertical-align: middle;">
							<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../userOtherInfo/userOtherInfo?type=${userModuleMenu==0?'0':'1'}`">
								<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 27rpx;">{{userModuleMenu==0?'添加用户':'添加管理员'}} </text>
								</view>
							</navigator>
							
							<view @click="deleteUserBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
								<view style="display: inline-block;vertical-align: middle;">
									<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
								</view>
								<text style="vertical-align: middle;font-size: 27rpx;">批量删除</text>
							</view>
							
						</view>
						<view class="content">
							<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="userTableSelectChange">
								<!-- 表头行 -->
								<uni-tr>
									<uni-th align="center" >操作</uni-th>
									<uni-th align="center" @filter-change="userUsernameFilter" filter-type="search">用户名</uni-th>
									<uni-th align="center" @filter-change="userPasswordFilter" filter-type="search">密码</uni-th>
									<uni-th align="center" @filter-change="userNameFilter" filter-type="search">昵称</uni-th>
									<uni-th align="center" >头像</uni-th>
									<uni-th align="center" @filter-change="userSexFilter" filter-type="select" :filter-data="userSexFilterData"  >性别</uni-th>
									<uni-th align="center" @filter-change="userBirthDayFilter" filter-type="date">生日</uni-th>
									<uni-th align="center" :sortable="true" @sort-change="userAgeSort">年龄</uni-th>
									<uni-th align="center" @filter-change="userPhoneFilter" filter-type="search">电话</uni-th>
									<uni-th align="center" @filter-change="userEmailFilter" filter-type="search">邮箱</uni-th>
									<uni-th align="center" :sortable="true" @sort-change="userTimeSort">注册时间</uni-th>
								</uni-tr>
								<!-- 表格数据行 -->
								<template v-for="(item,index) in users" :key="index">
									<uni-tr>
										<uni-td align="center">
											<view @click="deleteUserSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
												<uni-icons type="trash-filled" size="20"></uni-icons>
											</view>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../userOtherInfo/userOtherInfo?uid=${item.id}&type=${userModuleMenu==0?'0':'1'}}`">
												<view style="display: inline-block;vertical-align: middle">
													<uni-icons type="compose" size="20"></uni-icons>
												</view>
											</navigator>
											
											<view @click="userPermissionChange(index)" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
												<uni-icons :type="userModuleMenu==0?'personadd-filled':'person-filled'" size="20"></uni-icons>
											</view>
										</uni-td>
										<uni-td align="center">{{item.username}}</uni-td>
										<uni-td align="center">{{item.password}}</uni-td>
										<uni-td align="center">{{item.name}}</uni-td>
										<uni-td align="center">
												<image @error="basicUsersImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.headimage"></image>
										</uni-td>
										<uni-td align="center">
											<view :style="`color:${item.sex==0?'rgb(0, 137, 206)':item.sex==1?'rgb(230, 0, 0)':'rgb(27, 27, 27)'};`">
												{{item.sex==0?'男':item.sex==1?'女':'保密'}}
											</view>
										</uni-td>
										<uni-td align="center">{{item.birthday}}</uni-td>
										<uni-td align="center">{{item.age}}岁</uni-td>
										<uni-td align="center">{{item.phone}}</uni-td>
										<uni-td align="center">{{item.email}}</uni-td>
										<uni-td align="center">{{item.createtime.replace("T"," ")}}</uni-td>
									</uni-tr>
								</template>		
							</uni-table>
							<view>
								<uni-pagination  :total="userCondition.total" @change="clickUserPage"   :current="userCondition.current" :pageSize="userCondition.size"></uni-pagination>
							</view>
						</view>
						<!-- 用户管理view end -->
					</view>
					<view v-if="menu==3" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 词库管理view start -->
						<uni-popup ref="cikutypepopup"  type="center" @maskClick="cancelCikutypePop">
							<view style="background-color: black;padding: 20rpx;">
								<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
									{{cikutypeModal=="insert"?'添加词库类别':'修改词库类别'}}
								</view>
								<uni-forms ref="cikutypeEntity" :rules="cikuTypeRules" :modelValue="cikutypeEntity">
									<uni-forms-item name="dsc">
										<uni-easyinput
										@blur="binddata('dsc', cikutypeEntity.dsc,'cikutypeEntity')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="wallet-filled"
											height="90rpx"
											type="text"
											v-model="cikutypeEntity.dsc"
											
											placeholder="请输入词库类别名称"
										/>
									</uni-forms-item>
								</uni-forms>
								<view style="justify-content: center;text-align: center;">
									
									<button @click="insertCikuType" v-if="cikutypeModal=='insert'" type="primary"  size="mini">添加</button>
									<button @click="updateCikuType" v-else type="primary"   size="mini">修改</button>
									<button @click="cancelCikutypePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
								</view>
								
							</view>
						</uni-popup>
						<view style="padding: 20rpx;">
							<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 70%;display: inline-block;vertical-align: middle;">
								<template v-for="(item, index) in cikuModuleMenuTexts" :key="index">
									<view @tap="onClickCikuModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', cikuModuleMenu == index ? 'activeText' : '']">{{ item.dsc }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
							</scroll-view>
							<view style="display: inline-block;vertical-align: middle;width:30%;border-bottom: 1rpx solid white;padding-bottom: 33rpx;">
								<view style="justify-content: right;text-align: right;vertical-align: middle;">
						
										<view @click="insertCikuTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
											<view style="display: inline-block;vertical-align: middle;">
												<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
											</view>
											<text style="vertical-align: middle;font-size: 27rpx;">添加词库类别 </text>
										</view>
									
									
									
										<view @click="updateCikuTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
											<view style="display: inline-block;vertical-align: middle;">
												<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
											</view>
											<text style="vertical-align: middle;font-size: 27rpx;">修改该类别 </text>
										</view>
									
									<view @click="deleteCikuType" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
										<view style="display: inline-block;vertical-align: middle;">
											<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
										</view>
										<text style="vertical-align: middle;font-size: 27rpx;">删除该类别</text>
									</view>
									
								</view>
							</view>
						</view>
						<view style="justify-content: right;text-align: right;padding: 20rpx;vertical-align: middle;">
							<navigator v-if="cikuModuleMenuTexts.length!=0" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../importCiKu/importCiKu?cikutypeId=${cikuModuleMenuTexts.length!=0? cikuModuleMenuTexts[cikuModuleMenu].id:0}`">
								<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 27rpx;">添加词库 </text>
								</view>
							</navigator>
							
							<view @click="deleteCikuBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
								<view style="display: inline-block;vertical-align: middle;">
									<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
								</view>
								<text style="vertical-align: middle;font-size: 27rpx;">批量删除</text>
							</view>
							
						</view>
						<view class="content">
							<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="cikuTableSelectChange">
								<!-- 表头行 -->
								<uni-tr>
									<uni-th align="center"  >操作</uni-th>
									<uni-th align="center" @filter-change="cikuDscFilter" filter-type="search">名称</uni-th>
									<uni-th align="center"  @filter-change="cikuDscFilter" filter-type="search">简写</uni-th>
									<uni-th align="center" >单词量</uni-th>
									<uni-th align="center"  @filter-change="cikuUserFilter" :filter-data="cikuUserFilterData" filter-type="select">创建者</uni-th>
									<uni-th align="center" :sortable="true" @sort-change="cikuTimeSort">创建时间</uni-th>
								</uni-tr>
								<!-- 表格数据行 -->
								<template v-for="(item,index) in cikus" :key="index">
									<uni-tr>
										<uni-td align="center">
											<view @click="deleteCikuSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
												<uni-icons type="trash-filled" size="20"></uni-icons>
											</view>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../importCiKu/importCiKu?cikutypeId=${cikuModuleMenuTexts.length!=0? cikuModuleMenuTexts[cikuModuleMenu].id:0}&cikuId=${item.ciku.id!=undefined?item.ciku.id:0}`">
												<view style="display: inline-block;vertical-align: middle">
													<uni-icons type="compose" size="20"></uni-icons>
												</view>
											</navigator>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../cikuDetail/cikuDetail?cikutypeId=${cikuModuleMenuTexts.length!=0? cikuModuleMenuTexts[cikuModuleMenu].id:0}&cikuId=${item.ciku.id!=undefined?item.ciku.id:0}`">
											<view  style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
												<uni-icons type="font" size="20"></uni-icons>
											</view>
											</navigator>
										</uni-td>
										<uni-td align="center">{{item.ciku.dsc}}</uni-td>
										<uni-td align="center">{{item.ciku.dscabb}}</uni-td>
										<uni-td align="center">{{item.wordCount}}个</uni-td>
										<uni-td align="center">
											<view style="vertical-align: middle;">
												<view style="display: inline-block;vertical-align: middle;">
													{{item.user.username}}
												</view>
												<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
													<image @error="basicCikusImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
												</view>
												
											</view>
												
										</uni-td>
										<uni-td align="center">{{item.ciku.createtime.replace("T"," ")}}</uni-td>
									</uni-tr>
								</template>		
							</uni-table>
							<view>
								<uni-pagination  :total="cikuCondition.total" @change="clickCikuPage"   :current="cikuCondition.current" :pageSize="cikuCondition.size"></uni-pagination>
							</view>
						</view>
						<!-- 词库管理view end -->
					</view>
					<view v-if="menu==4" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 单词管理view start -->
						<view style="padding: 20rpx;">
							<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 100%;display: inline-block;vertical-align: middle;">
								<template v-for="(item, index) in wordModuleMenuTexts" :key="index">
									<view @tap="onClickWordModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', wordModuleMenu == index ? 'activeText' : '']">{{ item.text.toUpperCase() }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
							</scroll-view>
						</view>
						<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
							<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../wordDetail/wordDetail?initial=${wordModuleMenuTexts.length!=0?wordModuleMenuTexts[wordModuleMenu].text.toLowerCase():''}&current=${wordCondition.current}`">
								<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 10rpx;border-radius: 10rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="plus" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">添加单词 </text>
								</view>
							</navigator>
							
							<view @click="deleteWordBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
								<view style="display: inline-block;vertical-align: middle;">
									<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
								</view>
								<text style="vertical-align: middle;font-size: 25rpx;">批量删除</text>
							</view>
							
						</view>
						<view class="content">
							<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="wordTableSelectChange">
								<!-- 表头行 -->
								<uni-tr>
									<uni-th width="100" align="center">操作</uni-th>
									<uni-th width="100" align="center">词性</uni-th>
									<uni-th width="150" align="center" @filter-change="wordWordFilter" filter-type="search">单词</uni-th>
									<uni-th width="200" align="center" @filter-change="wordTransFilter" filter-type="search">译文</uni-th>
									<uni-th  width="100" align="center"  >音标</uni-th>
									<uni-th align="center" >例句</uni-th>
									<uni-th width="100" align="center" :sortable="true" @sort-change="wordTimeSort">创建时间</uni-th>
								</uni-tr>
								<!-- 表格数据行 -->
								<template v-for="(item,index) in words" :key="index">
									<uni-tr align="center">
										<uni-td align="center">
											<view @click="deleteWordSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
												<uni-icons type="trash-filled" size="20"></uni-icons>
											</view>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../wordDetail/wordDetail?initial=${item.word.word.slice(0,1).toLowerCase()}&id=${item.word.id}&current=${wordCondition.current}`">
												<view style="display: inline-block;vertical-align: middle">
													<uni-icons type="compose" size="20"></uni-icons>
												</view>
											</navigator>
										</uni-td>
										
										<uni-td align="center">
											<scroll-view :scroll-y="true" style="height: 250rpx;">
												{{item.word.charac}}
											</scroll-view>
										</uni-td>
										<uni-td align="center"><text style="font-weight: bolder;font-size: 32rpx;">{{item.word.word}}</text></uni-td>
										<uni-td align="center">
											<scroll-view :scroll-y="true" style="height: 250rpx;">
												{{item.word.trans}}
												<view style="display: inline-block;margin-left: 10rpx;cursor: pointer;" @click="playChinese(item.word.trans)">
													<uni-icons type="headphones" color="grey" size="18"></uni-icons>
												
												</view>
											</scroll-view>
										</uni-td>
										<uni-td align="center">
											<view style="display: flex;flex-direction: row;flex-wrap: nowrap;">
												<view style="width: 50%;display: inline-block;font-size: 10rpx;color: grey;"  >
													/{{item.word.soundmark1}}/
													<view style="display: inline-block;cursor: pointer;" v-if="item.audioUKFileExist" @tap.stop="playUK(item.word.word)">
														<uni-icons type="headphones" color="grey" size="20"></uni-icons>
														UK	
													</view>
													
												</view> 
												<view  style="width: 50%;display: inline-block;font-size: 10rpx;color: grey;margin-left: 20rpx;"  >
													/{{item.word.soundmark2}}/
													<view  style="display: inline-block;cursor: pointer;" v-if="item.audioUSAFileExist" @tap.stop="playUSA(item.word.word)">
														<uni-icons type="headphones" color="grey" size="20"></uni-icons>
														USA	
													</view>
													
												</view> 
											</view>
											
										</uni-td>
										<uni-td align="center">
											<view style="vertical-align: middle;">
												<scroll-view :scroll-y="true" style="height: 250rpx;">
													<template v-for="(item2,index2) in item.liju.sentences" :key="index2">
														<view style="padding: 20rpx;" v-if="item2!=null&&item2!=''&&item2.english!=undefined&&item2.english!=null&&item2.english!=''">
															<view style="" >
																{{index2+1}}.  
																<template v-for="(item3,index3) in item2.split" :key="index3">
																	<view :style="`display:inline-block;margin-left:10rpx;${index3>=item2.styleIndexStart&&index3<item2.styleIndexEnd?'font-weight: bolder;font-size: 32rpx;color :rgb(255, 170, 0)':''}`">
																		{{item3}}
																	</view>
																</template>
																<view style="display: inline-block;cursor: pointer;" @click="playUKSentence(item2.english)">
																	<uni-icons type="headphones" color="grey" size="20"></uni-icons>
																	UK
																</view>
																<view style="display: inline-block;margin-left: 10rpx;cursor: pointer;" @click="playUSASentence(item2.english)">
																	<uni-icons type="headphones" color="grey" size="20"></uni-icons>
																	USA
																</view>
															</view>
															<view style="margin-top: 10rpx;">
																{{item2.chinese}}
																<view style="display: inline-block;margin-left: 10rpx;cursor: pointer;" @click="playChinese(item2.chinese)">
																	<uni-icons type="headphones" color="grey" size="18"></uni-icons>
																
																</view>
															</view>
															<view style="margin-top: 10rpx;">
																{{item2.dict}}
															</view>
														</view>
													</template>
												</scroll-view>
											
												
											</view>
												
										</uni-td>
										<uni-td align="center">{{item.word.createtime.replace("T"," ")}}</uni-td>
									</uni-tr>
								</template>		
							</uni-table>
							<view>
								<uni-pagination  :total="wordCondition.total" @change="clickWordPage"   :current="wordCondition.current" :pageSize="wordCondition.size"></uni-pagination>
							</view>
						</view>
						<!-- 单词管理view  end -->
					</view>
					<view v-if="menu==5" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 阅读管理view start -->
						<uni-popup ref="readtypepopup"  type="center" @maskClick="cancelReadtypePop">
							<view style="background-color: black;padding: 20rpx;">
								<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
									{{readtypeModal=="insert"?'添加阅读一级类别':'修改阅读一级类别'}}
								</view>
								<uni-forms ref="readtypeEntity" :rules="readTypeRules" :modelValue="readtypeEntity">
									<uni-forms-item name="dsc">
										<uni-easyinput
										@blur="binddata('dsc', readtypeEntity.dsc,'readtypeEntity')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="chatboxes-filled"
											height="90rpx"
											type="text"
											v-model="readtypeEntity.dsc"
											
											placeholder="请输入阅读一级类别名称"
										/>
									</uni-forms-item>
								</uni-forms>
								<view style="justify-content: center;text-align: center;">
									
									<button @click="insertReadType" v-if="readtypeModal=='insert'" type="primary"  size="mini">添加</button>
									<button @click="updateReadType" v-else type="primary"   size="mini">修改</button>
									<button @click="cancelReadtypePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
								</view>
								
							</view>
						</uni-popup>
						<uni-popup ref="readtypesubpopup"  type="center" @maskClick="cancelReadtypeSubPop">
							<view style="background-color: black;padding: 20rpx;">
								<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
									{{readtypeSubModal=="insert"?'添加阅读二级类别':'修改阅读二级类别'}}
								</view>
								<uni-forms ref="readtypeSubEntity" :rules="readTypeSubRules" :modelValue="readtypeSubEntity">
									<uni-forms-item name="dsc">
										<uni-easyinput
										@blur="binddata('dsc', readtypeSubEntity.dsc,'readtypeSubEntity')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="chatboxed-filled"
											height="90rpx"
											type="text"
											v-model="readtypeSubEntity.dsc"
											
											placeholder="请输入阅读二级类别名称"
										/>
									</uni-forms-item>
								</uni-forms>
								<view style="justify-content: center;text-align: center;">
									
									<button @click="insertReadTypeSub" v-if="readtypeSubModal=='insert'" type="primary"  size="mini">添加</button>
									<button @click="updateReadTypeSub" v-else type="primary"   size="mini">修改</button>
									<button @click="cancelReadtypeSubPop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
								</view>
								
							</view>
						</uni-popup>
						<view style="padding: 20rpx;">
							<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 60%;display: inline-block;vertical-align: middle;">
								<template v-for="(item, index) in readModuleMenuTexts" :key="index">
									<view @tap="onClickReadModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', readModuleMenu == index ? 'activeText' : '']">{{ item.dsc }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
							</scroll-view>
							<view style="display: inline-block;vertical-align: middle;width:40%;border-bottom: 1rpx solid white;padding-bottom: 23rpx;padding-top: 10rpx;">
									<view style="justify-content: right;text-align: right;vertical-align: middle;">
							
											<view @click="insertReadTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">添加阅读一级类别 </text>
											</view>
										
										
										
											<view @click="updateReadTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">修改该阅读一级类别 </text>
											</view>
										
										<view @click="deleteReadType" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
											<view style="display: inline-block;vertical-align: middle;">
												<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
											</view>
											<text style="vertical-align: middle;font-size: 27rpx;">删除该阅读一级类别</text>
										</view>
										
									</view>
							</view>
							
							<scroll-view class="scrollXTarBar" :scroll-x="true" :style="`width: 60%;display: inline-block;vertical-align: middle;${readSubModuleMenuTexts.length==0?'border: none;':''}`">
								<template v-if="readSubModuleMenuTexts.length!=0" v-for="(item, index) in readSubModuleMenuTexts" :key="index">
									<view @tap="onClickReadSubModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', readSubModuleMenu == index ? 'activeText' : '']" style="font-size: 27rpx;">{{ item.dsc }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
								<view v-else style="justify-content: center;text-align: center;">
									暂无二级类别
								</view>
							</scroll-view>
							
							<view style="display: inline-block;vertical-align: middle;width:40%;border-bottom: 1rpx solid white;padding-bottom: 24rpx;padding-top: 11rpx;">
									<view style="justify-content: right;text-align: right;vertical-align: middle;">
							
											<view @click="insertReadTypeSubPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="18"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 21rpx;">添加阅读二级类别 </text>
											</view>
										
										
										
											<view v-if="readSubModuleMenuTexts.length!=0" @click="updateReadTypeSubPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="18"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 21rpx;">修改该阅读二级类别 </text>
											</view>
										
										<view v-if="readSubModuleMenuTexts.length!=0" @click="deleteReadTypeSub" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
											<view style="display: inline-block;vertical-align: middle;">
												<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="18"></uni-icons>
											</view>
											<text style="vertical-align: middle;font-size: 21rpx;">删除该阅读二级类别</text>
										</view>
										
									</view>
							</view>
						</view>
						
						<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
							<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../readDetail/readDetail?readType=${readModuleMenuTexts.length>0?readModuleMenuTexts[readModuleMenu].id:''}&readTypeSub=${readSubModuleMenuTexts.length>0?readSubModuleMenuTexts[readSubModuleMenu].id:''}&current=${readCondition.current}`">
								<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 10rpx;border-radius: 10rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="plus" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">添加文章 </text>
								</view>
							</navigator>
							
							<view @click="deleteReadBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
								<view style="display: inline-block;vertical-align: middle;">
									<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
								</view>
								<text style="vertical-align: middle;font-size: 25rpx;">批量删除</text>
							</view>
							
						</view>
						<view class="content">
							<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="readTableSelectChange">
								<!-- 表头行 -->
								<uni-tr>
									<uni-th width="100" align="center">操作</uni-th>
									<uni-th width="150" align="center" @filter-change="readNameFilter" filter-type="search">标题</uni-th>
									<uni-th width="100" align="center" @filter-change="readAuthorFilter" filter-type="search">作者</uni-th>
									<uni-th width="150" align="center" @filter-change="readBriefFilter" filter-type="search">简介</uni-th>
									<uni-th  width="100" align="center"  >封面</uni-th>
									<uni-th width="270" align="center" @filter-change="readEssayFilter" filter-type="search">文章</uni-th>
									<uni-th width="80" align="center" :sortable="true" @sort-change="readTimeSort">创建时间</uni-th>
								</uni-tr>
								<!-- 表格数据行 -->
								<template v-for="(item,index) in reads" :key="index">
									<uni-tr align="center">
										<uni-td align="center">
											<view @click="deleteReadSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
												<uni-icons type="trash-filled" size="20"></uni-icons>
											</view>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../readDetail/readDetail??readType=${readModuleMenuTexts.length>0?readModuleMenuTexts[readModuleMenu].id:''}&readTypeSub=${readSubModuleMenuTexts.length>0?readSubModuleMenuTexts[readSubModuleMenu].id:''}&id=${item.id}&current=${readCondition.current}`">
												<view style="display: inline-block;vertical-align: middle">
													<uni-icons type="compose" size="20"></uni-icons>
												</view>
											</navigator>
										</uni-td>
										<uni-td align="center" style="font-weight: bolder;">
											{{item.name}}
										</uni-td>
										<uni-td align="center" style="font-style: oblique;">
											{{item.author}}
										</uni-td>
										<uni-td>
											<scroll-view :scroll-y="true" style="height: 230rpx;text-indent: 1rem;">
												{{item.brief}}
											</scroll-view>												
										</uni-td>
										<uni-td align="center">
											<image @error="basicReadsImageError(index)" :src="item.image" style="width: 80%;height: 200rpx;"></image>
												
										</uni-td>
										<uni-td >
											<scroll-view :scroll-y="true" style="height: 160rpx;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
		word-spacing:3rpx;padding: 20rpx;line-height: 55rpx;font-size: 33rpx;">
												<template v-for="(item2,index2) in item.sentences" :key="index2">
													<view style="display: inline-block;position: relative;margin-bottom:15rpx;text-indent: 1rem;">
														{{item2}}
													</view>
													<view style="display: inline-block;cursor: pointer;" @click="playUKSentence(item2)">
														<uni-icons type="headphones" color="grey" size="18"></uni-icons>
													</view>
												</template>
											</scroll-view>				
										</uni-td>   
										<uni-td align="center">{{item.createtime.replace("T"," ")}}</uni-td>
									</uni-tr>
								</template>		
							</uni-table>
							<view>
								<uni-pagination  :total="readCondition.total" @change="clickReadPage"   :current="readCondition.current" :pageSize="readCondition.size"></uni-pagination>
							</view>
						</view>
						<!-- 阅读管理view end -->
					</view>
					<view v-if="menu==6" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!--  题库管理 view start -->
						<uni-popup ref="testtypepopup"  type="center" @maskClick="cancelTesttypePop">
							<view style="background-color: black;padding: 20rpx;">
								<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
									{{testtypeModal=="insert"?'添加题库':'修改题库'}}
								</view>
								<uni-forms ref="testtypeEntity" :rules="testTypeRules" :modelValue="testtypeEntity">
									<uni-forms-item name="dsc">
										<uni-easyinput
										@blur="binddata('dsc', testtypeEntity.dsc,'testtypeEntity')"
											placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
											color="white"
											fontSize="35rpx"
											prefixIcon="compose"
											height="90rpx"
											type="text"
											v-model="testtypeEntity.dsc"
											
											placeholder="请输入题库名称"
										/>
									</uni-forms-item>
								</uni-forms>
								<view style="justify-content: center;text-align: center;">
									
									<button @click="insertTestType" v-if="testtypeModal=='insert'" type="primary"  size="mini">添加</button>
									<button @click="updateTestType" v-else type="primary"   size="mini">修改</button>
									<button @click="cancelTesttypePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
								</view>
								
							</view>
						</uni-popup>
						<view style="padding: 20rpx;">
							<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 75%;display: inline-block;vertical-align: middle;">
								<template v-for="(item, index) in testModuleMenuTexts" :key="index">
									<view @tap="onClickTestModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
										<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
										<text :class="['scrollXTarBarText', testModuleMenu == index ? 'activeText' : '']">{{ item.dsc }}</text>
										<!-- </uni-transition> -->
									</view>
								</template>
							</scroll-view>
							<view style="display: inline-block;vertical-align: middle;width:25%;border-bottom: 1rpx solid white;padding-bottom: 23rpx;padding-top: 10rpx;">
									<view style="justify-content: right;text-align: right;vertical-align: middle;">
							
											<view @click="insertTestTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">添加题库 </text>
											</view>
										
										
										
											<view @click="updateTestTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">修改题库 </text>
											</view>
										
										<view @click="deleteTestType" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
											<view style="display: inline-block;vertical-align: middle;">
												<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
											</view>
											<text style="vertical-align: middle;font-size: 27rpx;">删除题库</text>
										</view>
										
									</view>
							</view>
						</view>
							
						<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
							<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../testDetail/testDetail?testType=${testModuleMenuTexts.length!=0?testModuleMenuTexts[testModuleMenu].id:''}&current=${testCondition.current}`">
								<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 10rpx;border-radius: 10rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="plus" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">添加题目 </text>
								</view>
							</navigator>
							
							<view @click="deleteTestBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
								<view style="display: inline-block;vertical-align: middle;">
									<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
								</view>
								<text style="vertical-align: middle;font-size: 25rpx;">批量删除</text>
							</view>
							
						</view>
						<view class="content">
							<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="testTableSelectChange">
								<!-- 表头行 -->
								<uni-tr>
									<uni-th width="100" align="center">操作</uni-th>
									<uni-th width="200" align="center" @filter-change="testTitleFilter" filter-type="search">题目</uni-th>
									<uni-th width="100" align="center" @filter-change="testOptaFilter" filter-type="search">选项<text style="color: red;font-weight: bolder;font-size: 33rpx;margin-left: 5rpx;">A</text></uni-th>
									<uni-th width="100" align="center" @filter-change="testOptbFilter" filter-type="search">选项<text style="color: blue;font-weight: bolder;font-size: 33rpx;margin-left: 5rpx;">B</text></uni-th>
									<uni-th width="100" align="center" @filter-change="testOptcFilter" filter-type="search">选项<text style="color:limegreen;font-weight: bolder;font-size: 33rpx;margin-left: 5rpx;">C</text></uni-th>
									<uni-th width="100" align="center" @filter-change="testOptdFilter" filter-type="search">选项<text style="color: darkorange;font-weight: bolder;font-size: 33rpx;margin-left: 5rpx;">D</text></uni-th>
									<uni-th  width="50" align="center" @filter-change="testAnswerFilter" :filterData="testAnswerFilterData" filter-type="select" >答案</uni-th>
									<uni-th width="150" align="center"  @filter-change="testAnalyFilter" filter-type="search">解析</uni-th>
									<uni-th width="100" align="center" :sortable="true" @sort-change="testTimeSort">创建时间</uni-th>
								</uni-tr>
								<!-- 表格数据行 -->
								<template v-for="(item,index) in tests" :key="index">
									<uni-tr align="center">
										<uni-td align="center">
											<view @click="deleteTestSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
												<uni-icons type="trash-filled" size="20"></uni-icons>
											</view>
											<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../testDetail/testDetail?testType=${testModuleMenuTexts.length!=0?testModuleMenuTexts[testModuleMenu].id:''}&id=${item.id}&current=${testCondition.current}`">
												<view style="display: inline-block;vertical-align: middle">
													<uni-icons type="compose" size="20"></uni-icons>
												</view>
											</navigator>
										</uni-td>
										
										<uni-td align="center">	
												<text style="text-indent: 1rem;-webkit-box-orient: vertical;word-break: break-all;;word-wrap: break-word;
		word-spacing:3rpx;padding: 20rpx;line-height: 55rpx;">{{item.title}}</text>
										</uni-td>
										<uni-td align="center">
											{{item.opta}}
										</uni-td>
										<uni-td align="center">
											{{item.optb}}
										</uni-td>
										<uni-td align="center">
											{{item.optc}}
										</uni-td>
										<uni-td align="center">
											{{item.optd}}
										</uni-td>
										<uni-td align="center">
											<text :style="`color: ${item.answer==0?'red':item.answer==1?'blue':item.answer==2?'limegreen':'darkorange'};font-weight: bolder;font-size: 33rpx;`">
												{{item.answer==0?'A':item.answer==1?'B':item.answer==2?'C':'D'}}
											</text>
											
										</uni-td>
										<uni-td align="center">
											<scroll-view :scroll-y="true" style="height: 260rpx;text-indent: 1rem;">
												{{item.analy}}
											</scroll-view>	
											
										</uni-td>
										<uni-td align="center">{{item.createtime.replace("T"," ")}}</uni-td>
									</uni-tr>
								</template>		
							</uni-table>
							<view>
								<uni-pagination  :total="testCondition.total" @change="clickTestPage"   :current="testCondition.current" :pageSize="testCondition.size"></uni-pagination>
							</view>
						</view>
						<!-- 题库管理 view end -->
					</view>
					<view v-if="menu==7" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 积分管理 view start -->
						<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 100%;display: inline-block;vertical-align: middle;">
							<template v-for="(item, index) in scoreModuleMenuTexts" :key="index">
								<view @tap="onClickScoreModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
									<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
									<text :class="['scrollXTarBarText', scoreModuleMenu == index ? 'activeText' : '']">{{ item }}</text>
									<!-- </uni-transition> -->
								</view>
							</template>
						</scroll-view>
						
						<!-- 积分来源管理 start -->
						<view style="padding: 20rpx;" v-if="scoreModuleMenu == 0">
							<view>					
									<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
										<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../scoreSourceDetail/scoreSourceDetail?current=${scoreSourceCondition.current}&index=${scoreModuleMenu}`">
											<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 10rpx;border-radius: 10rpx;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="plus" size="16"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 25rpx;">添加积分来源类别 </text>
											</view>
										</navigator>
									</view>
							</view>
							
							<view class="content">
									<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="scoreSourceTableSelectChange">
										<!-- 表头行 -->
										<uni-tr>
											<uni-th width="100" align="center">操作</uni-th>
											<uni-th width="100" align="center">id</uni-th>
											<uni-th width="200" align="center" @filter-change="scoreSourceDscFilter" filter-type="search">类别</uni-th>
											<uni-th width="100" align="center" :sortable="true" @sort-change="scoreSourceDefaultsSort">默认值</uni-th>

											<uni-th  width="50" align="center" @filter-change="scoreSourceTypeFilter" :filterData="scoreSourceTypeFilterData" filter-type="select" >加<text style="color: limegreen;margin-left: 5rpx;font-size: 40rpx;font-weight: bolder">+</text>/减<text style="color: red;margin-left: 5rpx;font-size: 40rpx;font-weight: bolder">-</text></uni-th>
											<uni-th width="100" align="center" :sortable="true" @sort-change="scoreSourceTimeSort">创建时间</uni-th>
										</uni-tr>
										<!-- 表格数据行 -->
										<template v-for="(item,index) in scoreSources" :key="index">
											<uni-tr align="center">
												
												<uni-td align="center">	
													<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../scoreSourceDetail/scoreSourceDetail?id=${item.id}&current=${scoreSourceCondition.current}&index=${scoreModuleMenu}`">
														<view style="display: inline-block;vertical-align: middle">
															<uni-icons type="compose" size="20"></uni-icons>
														</view>
													</navigator>
												</uni-td>
												<uni-td align="center">
													{{item.id}}
												</uni-td>
												<uni-td align="center">
													{{item.dsc}}
												</uni-td>
												<uni-td align="center">
													{{item.defaults==null||item.defaults==0?'无':`${item.type==0?'+':'-'}${item.defaults}分`}}
												</uni-td>
												<uni-td align="center">
													<text :style="`color: ${item.type==0?'limegreen':'red'};margin-left: 5rpx;font-size: 40rpx;font-weight: bolder`">
														{{item.type==0?'+':'-'}}
													</text>
													
												</uni-td>

												<uni-td align="center">{{item.createtime.replace("T"," ")}}</uni-td>
											</uni-tr>
										</template>		
									</uni-table>
									<view>
										<uni-pagination  :total="scoreSourceCondition.total" @change="clickScoreSourcePage"   :current="scoreSourceCondition.current" :pageSize="scoreSourceCondition.size"></uni-pagination>
									</view>
							</view>
						</view>
						<!-- 积分来源管理 end-->
						<!-- 积分管理 start -->
						<view style="padding: 20rpx;" v-if="scoreModuleMenu == 1">
							<uni-popup ref="scorepopup"  type="center" @maskClick="cancelScorePop">
								<view style="background-color: black;padding: 20rpx;">
									<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
										{{'修改积分'}}
									</view>
									<uni-forms ref="scoreEntity" :rules="scoreRules" :modelValue="scoreEntity">
										<uni-forms-item name="score">
											<uni-easyinput
											@blur="binddata('score', scoreEntity.score,'scoreEntity')"
												placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
												color="white"
												fontSize="35rpx"
												prefixIcon="smallcircle"
												height="90rpx"
												type="number"
												v-model="scoreEntity.score"
												
												placeholder="请输入积分数值"
											/>
										</uni-forms-item>
									</uni-forms>
									<view style="justify-content: center;text-align: center;">
										
										
										<button @click="updateScore"  type="primary"   size="mini">修改</button>
										<button @click="cancelScorePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
									</view>
									
								</view>
							</uni-popup>
							<view class="content">
									<uni-table border stripe  emptyText="暂无更多数据" >
										<!-- 表头行 -->
										<uni-tr>
											<uni-th  align="center">操作</uni-th>
											<uni-th align="center"  @filter-change="scoreUserFilter" :filter-data="scoreUserFilterData" filter-type="select">用户</uni-th>
											<uni-th  align="center" :sortable="true" @sort-change="scoreScoreSort">积分</uni-th>
						
											<uni-th  align="center" :sortable="true" @sort-change="scoreTimeSort">创建时间</uni-th>
										</uni-tr>
										<!-- 表格数据行 -->
										<template v-for="(item,index) in scores" :key="index">
											<uni-tr align="center">
												
												<uni-td align="center">	

														<view @click="updateScorePop(index)" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
															<uni-icons type="compose" size="20"></uni-icons>
														</view>
													
												</uni-td>
												<uni-td align="center">
													<view style="vertical-align: middle;">
														<view style="display: inline-block;vertical-align: middle;">
															{{item.user.username}}
														</view>
														<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
															<image @error="basicScoresImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
														</view>
														
													</view>
														
												</uni-td>
												<uni-td align="center">
													{{item.score.score}}
												</uni-td>
												
												<uni-td align="center">{{item.score.createtime.replace("T"," ")}}</uni-td>
											</uni-tr>
										</template>		
									</uni-table>
									<view>
										<uni-pagination  :total="scoreCondition.total" @change="clickScorePage"   :current="scoreCondition.current" :pageSize="scoreCondition.size"></uni-pagination>
									</view>
							</view>
						</view>
						<!-- 积分管理 end-->
						<!-- 流水管理 start -->
						<view style="padding: 20rpx;" v-if="scoreModuleMenu == 2">
							<uni-popup ref="zborderpopup"  type="center" @maskClick="cancelZbOrderPop">
								<view style="background-color: black;padding: 20rpx;">
									<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
										{{'充值金额'}}
									</view>
									<view style="vertical-align: middle;">
										
										<view class="titles1" style="vertical-align: middle;">
											用户：{{zbOrderEntity.user.username}}
										</view>
										<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
											<image @error="basicZbordersPopImageError" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="zbOrderEntity.user.headimage"></image>
										</view>
										
									</view>
									<uni-forms ref="zbOrderEntity" :rules="zbOrderRules" :modelValue="zbOrderEntity">
										<uni-forms-item name="money">
											<uni-easyinput
											@blur="binddata('money', zbOrderEntity.zborder.money,'zbOrderEntity')"
												placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
												color="white"
												fontSize="35rpx"
												prefixIcon="smallcircle"
												height="90rpx"
												type="number"
												v-model="zbOrderEntity.zborder.money"
												
												placeholder="请输入充值金额"
											/>
										</uni-forms-item>
										
									</uni-forms>
									<view style="padding: 50rpx;">
										充值{{zbOrderEntity.zborder.money==null?0:zbOrderEntity.zborder.money*10}}积分
									</view>
									<view style="justify-content: center;text-align: center;">
										
										
										<button @click="updateZbOrder"  type="primary"   size="mini">充值</button>
										<button @click="cancelZbOrderPop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
									</view>
									
								</view>
							</uni-popup>
							<view class="content">
									<uni-table border stripe  emptyText="暂无更多数据" >
										<!-- 表头行 -->
										<uni-tr>
											<uni-th width="100"  align="center">操作</uni-th>
											<uni-th width="250" align="center" @filter-change="zbOrderZbcodeFilter" filter-type="search">订单号</uni-th>
											<uni-th width="200" align="center" @filter-change="zbOrderZborderIdFilter" filter-type="search">支付宝流水号</uni-th>
											<uni-th width="150"  align="center"  @filter-change="zbOrderUserFilter" :filter-data="zbOrderUserFilterData" filter-type="select">用户</uni-th>
											<uni-th width="150" align="center" :sortable="true" @sort-change="zbOrderDscSort">充值积分</uni-th>
											<uni-th width="150" align="center" :sortable="true" @sort-change="zbOrderMoneySort">充值金额</uni-th>
											<uni-th width="100" align="center"  @filter-change="zbOrderStatusFilter" :filter-data="zbOrderStatusFilterData" filter-type="select">状态</uni-th>
											<uni-th width="100"  align="center" :sortable="true" @sort-change="zbOrderTimeSort">创建时间</uni-th>
										</uni-tr>
										<!-- 表格数据行 -->
										<template v-for="(item,index) in zborders" :key="index">
											<uni-tr align="center">
												
												<uni-td align="center">	
						
														<view @click="updateZbOrderPop(index)" style="display: inline-block;vertical-align: middle;margin-left: 20rpx;cursor: pointer;">
															<uni-icons type="compose" size="20"></uni-icons>充值
														</view>
													
												</uni-td>
												<uni-td align="center">
													{{item.zborder.zborderid}}
												</uni-td>
												<uni-td align="center">
													{{item.zborder.zbcode}}
												</uni-td>
												<uni-td align="center">
													<view style="vertical-align: middle;">
														<view class="titles1" style="vertical-align: middle;">
															{{item.user.username}}
														</view>
														<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
															<image @error="basicZbordersImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
														</view>
														
													</view>
														
												</uni-td>
												<uni-td align="center">
													{{item.zborder.dsc}}积分充值
												</uni-td>
												<uni-td align="center">
													{{item.zborder.money}}元
												</uni-td>
												<uni-td align="center">
													<view :style="`color:${item.zborder.status==0?'rgb(198, 0, 0)':item.zborder.status==1?'rgb(0, 214, 103)':'rgb(153, 153, 0)'}`">
														{{item.zborder.status==0?'未支付':item.zborder.status==1?'已支付':'已取消' }}
														
													</view>
												</uni-td>
												<uni-td align="center">{{item.zborder.createtime.replace("T"," ")}}</uni-td>
											</uni-tr>
										</template>		
									</uni-table>
									<view>
										<uni-pagination  :total="zbOrderCondition.total" @change="clickZbOrderPage"   :current="zbOrderCondition.current" :pageSize="zbOrderCondition.size"></uni-pagination>
									</view>
							</view>
						</view>
						<!-- 流水管理 end-->
						<!-- 积分管理 view end -->
					</view>
					<view v-if="menu==8" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 商品管理view start -->
							<uni-popup ref="ptypepopup"  type="center" @maskClick="cancelPtypePop">
								<view style="background-color: black;padding: 20rpx;">
									<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
										{{ptypeModal=="insert"?'添加商品一级类别':'修改商品一级类别'}}
									</view>
									<uni-forms ref="ptypeEntity" :rules="pTypeRules" :modelValue="ptypeEntity">
										<uni-forms-item name="dsc">
											<uni-easyinput
											@blur="binddata('dsc', ptypeEntity.dsc,'ptypeEntity')"
												placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
												color="white"
												fontSize="35rpx"
												prefixIcon="gift-filled"
												height="90rpx"
												type="text"
												v-model="ptypeEntity.dsc"
												
												placeholder="请输入商品一级类别名称"
											/>
										</uni-forms-item>
									</uni-forms>
									<view style="justify-content: center;text-align: center;">
										
										<button @click="insertPType" v-if="ptypeModal=='insert'" type="primary"  size="mini">添加</button>
										<button @click="updatePType" v-else type="primary"   size="mini">修改</button>
										<button @click="cancelPtypePop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
									</view>
									
								</view>
							</uni-popup>
							<uni-popup ref="ptypesubpopup"  type="center" @maskClick="cancelPtypeSubPop">
								<view style="background-color: black;padding: 20rpx;">
									<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
										{{ptypeSubModal=="insert"?'添加商品二级类别':'修改商品二级类别'}}
									</view>
									<uni-forms ref="ptypeSubEntity" :rules="pTypeSubRules" :modelValue="ptypeSubEntity">
										<uni-forms-item name="dsc">
											<uni-easyinput
											@blur="binddata('dsc', ptypeSubEntity.dsc,'ptypeSubEntity')"
												placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
												color="white"
												fontSize="35rpx"
												prefixIcon="gift-filled"
												height="90rpx"
												type="text"
												v-model="ptypeSubEntity.dsc"
												
												placeholder="请输入商品二级类别名称"
											/>
										</uni-forms-item>
									</uni-forms>
									<view style="justify-content: center;text-align: center;">
										
										<button @click="insertPTypeSub" v-if="ptypeSubModal=='insert'" type="primary"  size="mini">添加</button>
										<button @click="updatePTypeSub" v-else type="primary"   size="mini">修改</button>
										<button @click="cancelPtypeSubPop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
									</view>
									
								</view>
							</uni-popup>
							<view style="padding: 20rpx;">
								<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 60%;display: inline-block;vertical-align: middle;">
									<template v-for="(item, index) in productModuleMenuTexts" :key="index">
										<view @tap="onClickProductModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
											<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
											<text :class="['scrollXTarBarText', productModuleMenu == index ? 'activeText' : '']">{{ item.dsc }}</text>
											<!-- </uni-transition> -->
										</view>
									</template>
								</scroll-view>
								<view style="display: inline-block;vertical-align: middle;width:40%;border-bottom: 1rpx solid white;padding-bottom: 23rpx;padding-top: 10rpx;">
										<view style="justify-content: right;text-align: right;vertical-align: middle;">
								
												<view @click="insertPTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 27rpx;">添加商品一级类别 </text>
												</view>
											
											
											
												<view @click="updatePTypePop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 27rpx;">修改该商品一级类别 </text>
												</view>
											
											<view @click="deletePType" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">删除该商品一级类别</text>
											</view>
											
										</view>
								</view>
								
								<scroll-view class="scrollXTarBar" :scroll-x="true" :style="`width: 60%;display: inline-block;vertical-align: middle;${productSubModuleMenuTexts.length==0?'border: none;':''}`">
									<template v-if="productSubModuleMenuTexts.length!=0" v-for="(item, index) in productSubModuleMenuTexts" :key="index">
										<view @tap="onClickProductSubModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
											<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
											<text :class="['scrollXTarBarText', productSubModuleMenu == index ? 'activeText' : '']" style="font-size: 27rpx;">{{ item.dsc }}</text>
											<!-- </uni-transition> -->
										</view>
									</template>
									<view v-else style="justify-content: center;text-align: center;">
										暂无二级类别
									</view>
								</scroll-view>
								
								<view style="display: inline-block;vertical-align: middle;width:40%;border-bottom: 1rpx solid white;padding-bottom: 24rpx;padding-top: 11rpx;">
										<view style="justify-content: right;text-align: right;vertical-align: middle;">
								
												<view @click="insertPTypeSubPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="18"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 21rpx;">添加商品二级类别 </text>
												</view>
											
											
											
												<view v-if="productSubModuleMenuTexts.length!=0" @click="updatePTypeSubPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="18"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 21rpx;">修改该商品二级类别 </text>
												</view>
											
											<view v-if="productSubModuleMenuTexts.length!=0" @click="deletePTypeSub" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="18"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 21rpx;">删除该商品二级类别</text>
											</view>
											
										</view>
								</view>
							</view>
							
							<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
								<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../productDetail/productDetail?pType=${productModuleMenuTexts.length>0?productModuleMenuTexts[productModuleMenu].id:''}&pTypeSub=${productSubModuleMenuTexts.length>0?productSubModuleMenuTexts[productSubModuleMenu].id:''}&current=${productCondition.current}`">
									<view style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 10rpx;border-radius: 10rpx;">
										<view style="display: inline-block;vertical-align: middle;">
											<uni-icons color="rgb(245, 245, 245)" type="plus" size="16"></uni-icons>
										</view>
										<text style="vertical-align: middle;font-size: 25rpx;">添加商品 </text>
									</view>
								</navigator>
								
								<view @click="deleteProductBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">批量删除</text>
								</view>
								
							</view>
							<view class="content">
								<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="productTableSelectChange">
									<!-- 表头行 -->
									<uni-tr>
										<uni-th width="100" align="center">操作</uni-th>
										<uni-th width="150" align="center" @filter-change="productTitleFilter" filter-type="search">商品名</uni-th>
										<uni-th width="100" align="center" :sortable="true" @sort-change="productPriceSort" >价格</uni-th>
										<uni-th width="100" align="center" :sortable="true" @sort-change="productNumSort" >库存</uni-th>
										<uni-th width="100" align="center" >商品图片</uni-th>
										<uni-th width="150"  align="center"  @filter-change="productMerchantFilter" :filter-data="productMerchantFilterData" filter-type="select">商家</uni-th>
										
										
										<uni-th width="80" align="center" :sortable="true" @sort-change="productTimeSort">创建时间</uni-th>
									</uni-tr>
									<!-- 表格数据行 -->
									<template v-for="(item,index) in products" :key="index">
										<uni-tr align="center">
											<uni-td align="center">
												<view @click="deleteProductSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
													<uni-icons type="trash-filled" size="20"></uni-icons>
												</view>
												<navigator style="display: inline-block;vertical-align: middle;margin-left: 20rpx;" :url="`../productDetail/productDetail??pType=${productModuleMenuTexts.length>0?productModuleMenuTexts[productModuleMenu].id:''}&pTypeSub=${productSubModuleMenuTexts.length>0?productSubModuleMenuTexts[productSubModuleMenu].id:''}&id=${item.product.id}&current=${productCondition.current}`">
													<view style="display: inline-block;vertical-align: middle">
														<uni-icons type="compose" size="20"></uni-icons>
													</view>
												</navigator>
											</uni-td>
											<uni-td align="center" style="font-weight: bolder;">
												<scroll-view :scroll-y="true" style="height: 210rpx;">
													{{item.product.title}}
												</scroll-view>
												
											</uni-td>
											<uni-td align="center" style="font-style: oblique;">
												{{item.product.price}}积分
											</uni-td>
											<uni-td align="center">
													{{item.product.num}}件										
											</uni-td>
											<uni-td align="center">
												<image @error="basicProductsImageError(index)" :src="item.product.imgAddre" style="width: 80%;height: 200rpx;"></image>
													
											</uni-td>
											<uni-td align="center">
												<view style="vertical-align: middle;">
													<view class="titles1" style="vertical-align: middle;">
														{{item.user.username}}
													</view>
													<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
														<image @error="basicProductsUserImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
													</view>
													
												</view>			
											</uni-td>   
											<uni-td align="center">{{item.product.time.replace("T"," ")}}</uni-td>
										</uni-tr>
									</template>		
								</uni-table>
								<view>
									<uni-pagination  :total="productCondition.total" @change="clickProductPage"   :current="productCondition.current" :pageSize="productCondition.size"></uni-pagination>
								</view>
							</view>
						<!-- 商品管理view end -->
					</view>
					<view v-if="menu==9" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!--  订单管理 view start -->
							<uni-popup ref="orderstatuspopup"  type="center" @maskClick="cancelOrderstatusPop">
								<view style="background-color: black;padding: 20rpx;">
									<view style="justify-content: center;text-align: center;margin-bottom: 20rpx;">
										{{orderstatusModal=="insert"?'添加订单状态':'修改订单状态'}}
									</view>
									<uni-forms ref="orderstatusEntity" :rules="orderStatusRules" :modelValue="orderstatusEntity">
										<uni-forms-item name="dsc">
											<uni-easyinput
											@blur="binddata('dsc', orderstatusEntity.dsc,'orderstatusEntity')"
												placeholderStyle="color:rgb(234, 234, 234);font-size:35rpx"
												color="white"
												fontSize="35rpx"
												prefixIcon="calendar-filled"
												height="90rpx"
												type="text"
												v-model="orderstatusEntity.dsc"
												
												placeholder="请输入订单状态名称"
											/>
										</uni-forms-item>
									</uni-forms>
									<view style="justify-content: center;text-align: center;">
										
										<button @click="insertOrderStatus" v-if="orderstatusModal=='insert'" type="primary"  size="mini">添加</button>
										<button @click="updateOrderStatus" v-else type="primary"   size="mini">修改</button>
										<button @click="cancelOrderstatusPop" style="margin-left: 20rpx;" type="warn" size="mini">取消</button>
									</view>
									
								</view>
							</uni-popup>
							<view style="padding: 20rpx;">
								<scroll-view class="scrollXTarBar" :scroll-x="true" style="width: 65%;display: inline-block;vertical-align: middle;">
									<template v-for="(item, index) in orderModuleMenuTexts" :key="index">
										<view @tap="onClickOrderModuleMenuItem(index)" :data-currentIndex="index" class="scrollXTarBarContent">
											
												<text :class="['scrollXTarBarText', orderModuleMenu == index ? 'activeText' : '']">{{ item.dsc }}</text>
											<text :class="['scrollXTarBarText', orderModuleMenu == index ? 'activeText' : '']" style="padding-left: 5rpx;">({{ordersNums[index]}})</text>
											<!-- <uni-transition ref="ani" :mode-class="['fade']" :show="animateByText"> -->
											
											<!-- </uni-transition> -->
										</view>
									</template>
								</scroll-view>
								<view style="display: inline-block;vertical-align: middle;width:35%;border-bottom: 1rpx solid white;padding-bottom: 23rpx;padding-top: 10rpx;">
										<view style="justify-content: right;text-align: right;vertical-align: middle;">
								
												<view @click="insertOrderStatusPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #008acf, #005a87);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 27rpx;">添加订单状态 </text>
												</view>
											
											
											
												<view @click="updateOrderStatusPop" style="margin-right: 20rpx;display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #009347, #3fbe5d);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
													<view style="display: inline-block;vertical-align: middle;">
														<uni-icons color="rgb(245, 245, 245)" type="plus" size="20"></uni-icons>
													</view>
													<text style="vertical-align: middle;font-size: 27rpx;">修改订单状态 </text>
												</view>
											
											<view @click="deleteOrderStatus" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 15rpx;border-radius: 10rpx;cursor: pointer;">
												<view style="display: inline-block;vertical-align: middle;">
													<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="20"></uni-icons>
												</view>
												<text style="vertical-align: middle;font-size: 27rpx;">删除订单状态</text>
											</view>
											
										</view>
								</view>
							</view>
								
							<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
								
								<view @click="sendOutOrderBatch" v-if="orderModuleMenu==0" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #55aa7f, #4f9fef);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="upload-filled" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">批量发货</text>
								</view>
								<view @click="receiveOrderBatch" v-if="orderModuleMenu==1" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #55aa7f, #4f9fef);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="checkmarkempty" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">批量收货</text>
								</view>
								<view @click="cancelOrderBatch" v-if="orderModuleMenu!=2&&orderModuleMenu!=3" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;margin-left: 25rpx;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">批量取消</text>
								</view>
								
							</view>
							<view class="content">
								<scroll-view :scroll-y="true" style="height: 66vh;">
									<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="orderTableSelectChange">
										<!-- 表头行 -->
										<uni-tr>
											<uni-th v-if="!(orderModuleMenu==2||orderModuleMenu==3)" width="100" align="center">操作</uni-th>
											<uni-th  align="center" @filter-change="orderOrdersFilter" filter-type="search">订单号</uni-th>
											<uni-th width="200" align="center" @filter-change="orderProductFilter" :filterData="orderProductFilterData" filter-type="select">商品</uni-th>
											<uni-th width="200" align="center" >联系方式</uni-th>
											<uni-th  width="300" align="center" v-if="orderModuleMenu==1||orderModuleMenu==2">物流状态</uni-th>
											<uni-th width="100" align="center" >订单状态</uni-th>
											<uni-th  width="100" align="center" @filter-change="orderUserFilter" :filterData="orderUserFilterData" filter-type="select" >购买用户</uni-th>
											
											<uni-th width="100" align="center" :sortable="true" @sort-change="orderTimeSort">创建时间</uni-th>
										</uni-tr>
										<!-- 表格数据行 -->
										<template v-for="(item,index) in orders" :key="index">
											<uni-tr align="center">
												<uni-td align="center" v-if="!(orderModuleMenu==2||orderModuleMenu==3)">
													
													<view @click="sendOutOrderSingle(index)" v-if="orderModuleMenu==0" style="display: inline-block;vertical-align: middle;cursor: pointer;">
														<uni-icons type="upload-filled" size="20"></uni-icons>
													</view>
													<view @click="receiveOrderSingle(index)" v-else-if="orderModuleMenu==1" style="display: inline-block;vertical-align: middle;cursor: pointer;">
														<uni-icons type="checkmarkempty" size="20"></uni-icons>
													</view>
													<view @click="cancelOrderSingle(index)" v-if="orderModuleMenu!=2&&orderModuleMenu!=3" style="margin-left: 20rpx;display: inline-block;vertical-align: middle;cursor: pointer;">
														<uni-icons type="trash-filled" size="20"></uni-icons>
													</view>
												</uni-td>
												
												<uni-td align="center">
													
													<view>
														{{item.order.orders.slice(0,24)}}
													</view>
													<view>
														{{item.order.orders.slice(24)}}
													</view>
													
														
												</uni-td>
												<uni-td align="center">
													<view>
														<view>
															<image @error="basicOrdersProductImageError(index)" :src="item.product.product.imgAddre" style="width: 80%;height: 200rpx;"></image>
														</view>
														<view>
															<scroll-view :scroll-y="true" style="height: 210rpx;">
																商品名：{{item.product.product.title}}
															</scroll-view>
														</view>
														<view>
															价格：{{item.product.product.price}}积分
														</view>
														
														<view>
															<view style="vertical-align: middle;">
																<view class="titles1" style="vertical-align: middle;margin-bottom: 20rpx;">
																	商家：{{item.product.user.username}}
																</view>
																<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
																	<image @error="basicOrdersProductUserImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.product.user.headimage"></image>
																</view>
																
															</view>		
														</view>
													</view>
												</uni-td>
												<uni-td align="center">
													<view>
														地址：{{item.address.address}}
													</view>
													<view>
														电话：{{item.address.phone}}
													</view>
													<view>
														联系人：{{item.address.name}}
													</view>
												</uni-td>
												<uni-td align="center" v-if="orderModuleMenu==1||orderModuleMenu==2">
													<template v-if="item.logistics!=null" >
														<scroll-view :scroll-y="true" style="height: 600rpx;">
															<view>
																物流状态：{{item.logistics.result.deliverystatus=='0'?'快递收件(揽件)':item.logistics.result.deliverystatus=='1'?'正在途中':item.logistics.result.deliverystatus=='2'?'正在派件':item.logistics.result.deliverystatus=='3'?'已签收':item.logistics.result.deliverystatus=='4'?'派送失败':item.logistics.result.deliverystatus=='5'?'疑难件':'退件签收'}}
															</view>
															<view style="vertical-align: middle;">
																<uni-link :href="'https://'+item.logistics.result.expSite">
																	<image :src="item.logistics.result.logo" style="display: inline-block;width: 50rpx;height: 50rpx;vertical-align: middle;"></image>{{item.logistics.result.expName}}
																	<text style="vertical-align: middle;">{{item.logistics.result.number}}</text>
																</uni-link>	
															</view>
															<view>
																<view>
																	{{item.logistics.result.courier==''?'':('快递员：'+item.logistics.result.courier)}}
																</view>
																<view>
																	{{item.logistics.result.courierPhone==''?'':('快递员电话：'+item.logistics.result.courierPhone)}}
																</view>
															</view>
															<view>
																<!-- <uni-steps :options="item.logistics.result.data" active-color="#007AFF" :active="item.logistics.result.data==undefined?0:item.logistics.result.data.length-1" direction="column" /> -->
																<uni-steps :options="item.logistics.result.data" active-color="#007AFF" :active="0" direction="column" />
															</view>
														</scroll-view>
														
													</template>
													<template v-else>
														<view style="justify-content: center;text-align: center;">
															暂未物流信息或物流信息错误
														</view>
														
													</template>
												</uni-td>
												<uni-td align="center">
													<template v-for="(item2,index2) in orderModuleMenuTexts" :key="index2">
														<view v-if="item2.id==item.order.orderstatus">
															{{item2.dsc}}
														</view>
													</template>
												</uni-td>
												<uni-td align="center">
													<view style="vertical-align: middle;">
														<view class="titles1" style="vertical-align: middle;">
															{{item.user.username}}
														</view>
														<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
															<image @error="basicOrdersUserImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
														</view>
														
													</view>		
												</uni-td>
												
												<uni-td align="center">{{item.order.createtime.replace("T"," ")}}</uni-td>
											</uni-tr>
										</template>		
									</uni-table>
								</scroll-view>
								
								<view style="margin-top: 10rpx;">
									<uni-pagination  :total="orderCondition.total" @change="clickOrderPage"   :current="orderCondition.current" :pageSize="orderCondition.size"></uni-pagination>
								</view>
							</view>
						<!-- 订单管理 view end -->
					</view>
					<view v-if="menu==10" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 数据管理 view start -->
						<uni-section  type="line" background="inherit" titleColor="white" titleFontSize="40rpx" :title="`数据管理`"></uni-section>
						<view style="padding: 50rpx;text-align: center;justify-content: center;display: flex;flex-direction: row;flex-wrap: wrap;margin-top:5%">
							<view style="width:50%">
								<template v-for="(item,index) in allDatas.slice(0,6)" :key="index">
									<view :style="`padding: 40rpx;vertical-align: middle;`">
										<view style="display: inline-block;vertical-align: middle;margin-right: 20rpx;">
											<uni-icons :type="item.icon" color="rgb(206, 206, 206)" size="15"></uni-icons>
										</view>
										
										<view style="display: inline-block;justify-content: left;text-align: left;width: 40%;font-size: 39rpx;font-style: oblique;vertical-align: middle;">{{item.dsc}}:</view>
										<view style="display: inline-block;justify-content: right;;text-align: right;width: 40%;font-size: 41rpx;font-weight: bolder;color: red;vertical-align: middle;">{{item.num}}</view>
										
									</view>
								</template>
							</view>
							<view style="width: 50%;">
								<template v-for="(item,index) in allDatas.slice(6,12)" :key="index">
									<view :style="`padding: 40rpx;vertical-align: middle;`">
										<view style="display: inline-block;vertical-align: middle;margin-right: 20rpx;">
											<uni-icons :type="item.icon" color="rgb(206, 206, 206)" size="15"></uni-icons>
										</view>
										
										<view style="display: inline-block;justify-content: left;text-align: left;width: 40%;font-size: 39rpx;font-style: oblique;vertical-align: middle;">{{item.dsc}}:</view>
										<view style="display: inline-block;justify-content: right;;text-align: right;width: 40%;font-size: 41rpx;font-weight: bolder;color: red;vertical-align: middle;">{{item.num}}</view>
										
									</view>
								</template>
							</view>
							<view>
								<template v-for="(item,index) in allDatas.slice(12,allDatas.length)" :key="index">
									<view :style="`padding: 20rpx;vertical-align: middle;`">
										<view style="display: inline-block;vertical-align: middle;margin-right: 20rpx;">
											<uni-icons :type="item.icon" color="rgb(206, 206, 206)" size="15"></uni-icons>
										</view>
										
										<view style="display: inline-block;justify-content: left;text-align: left;width: 10%;font-size: 37rpx;font-style: oblique;vertical-align: middle;">{{item.dsc}}:</view>
										<view style="display: inline-block;justify-content: right;;text-align: right;width: 10%;font-size: 39rpx;font-weight: bolder;color: red;vertical-align: middle;">{{item.num}}</view>
										
									</view>
								</template>
							</view>
						</view>
						<!-- 数据管理 view end -->
					</view>
					<view v-if="menu==11" style="background: linear-gradient(90deg, #0d0d0d, #595959);color: white;height:100vh;vertical-align: middle;">
						<!-- 学习圈管理end -->
						<view style="justify-content: right;text-align: right;padding: 15rpx;vertical-align: middle;">
								
								
								<view @click="deleteStudyCircleBatch" style="display: inline-block;vertical-align: middle;background: linear-gradient(90deg, #d30000, #cc4400);padding: 10rpx;border-radius: 10rpx;cursor: pointer;">
									<view style="display: inline-block;vertical-align: middle;">
										<uni-icons color="rgb(245, 245, 245)" type="trash-filled" size="16"></uni-icons>
									</view>
									<text style="vertical-align: middle;font-size: 25rpx;">批量删除</text>
								</view>
								
							</view>
							<view class="content">
								<uni-table border stripe type="selection" emptyText="暂无更多数据" @selection-change="studyCircleTableSelectChange">
									<!-- 表头行 -->
									<uni-tr>
										<uni-th width="50" align="center">操作</uni-th>
										<uni-th  align="center"  @filter-change="studyCircleTagsFilter" :filter-data="studyCircleTagsFilterData" filter-type="select">标签</uni-th>
										<uni-th width="100" align="center" @filter-change="studyCircleTitleFilter" filter-type="search" >标题</uni-th>
										<uni-th width="100" align="center"  >标题图片</uni-th>
										<uni-th width="100" align="center" >标题音频</uni-th>
										<uni-th width="200" align="center" >内容</uni-th>
			
										<uni-th width="100"  align="center"  @filter-change="studyCircleUserFilter" :filter-data="studyCircleUserFilterData" filter-type="select">发布者</uni-th>
										
										
										<uni-th width="80" align="center" :sortable="true" @sort-change="studyCircleTimeSort">创建时间</uni-th>
									</uni-tr>
									<!-- 表格数据行 -->
									<template v-for="(item,index) in studycircles" :key="index">
										<uni-tr align="center">
											<uni-td align="center">
												<view @click="deleteStudyCircleSignle(index)" style="display: inline-block;vertical-align: middle;cursor: pointer;">
													<uni-icons type="trash-filled" size="20"></uni-icons>
												</view>
											</uni-td>
											<uni-td align="center" style="">
												<view  class="tag-view" v-if="item.tags!=undefined&&item.tags.length!=0" style="padding:0 20rpx 20rpx 20rpx;width:150rpx;vertical-align: middle;">
													<template v-for="(item2,index2) in item.tags" :key="index2">
														<view style="display: inline-block;margin-right: 20rpx;"><uni-tag :text="'#'+item2.dsc" type="primary" size="small" /></view>
													</template>
												</view>
												<view v-else style="justify-content: center;text-align: center;">
													暂无内容
												</view>
											</uni-td>
											<uni-td align="center" style="font-weight: bolder;justify-content: center;text-align: center;">
												<scroll-view :scroll-y="true" style="height: 210rpx;justify-content: center;text-align: center;">
													{{item.studyCircle.title}}
												</scroll-view>
											</uni-td>
											<uni-td align="center">
												<scroll-view :scroll-y="true" style="height: 210rpx;" >
													<view style="display: flex;flex-wrap: wrap;margin-top: 20rpx;" v-if="item.studyCircle.image!=''">
														<template v-for="(item4, index4) in item.studyCircle.image.split(',')" :key="index4">
															<image
																@click="previewImage(globalData.url+'static/image/studycircle/'+item4)"
																
																:src="globalData.url+'static/image/studycircle/'+item4"
																style="border-radius: 10rpx ;width: 150rpx;height:150rpx;;background-color: #f1f1f1;margin: 2px 1.66%;"
															></image>
														</template>
													</view>
													<view v-else style="justify-content: center;text-align: center;">
														暂无内容
													</view>		
												</scroll-view>
											</uni-td>
											<uni-td align="center">
												<scroll-view :scroll-y="true" style="height: 210rpx;" >
												<view style="margin-top: 20rpx;" v-if="item.studyCircle.audio!=''">
													<template v-for="(item2,index2) in item.studyCircle.audio.split(',')" :key="index2">
														<view style="margin-top: 20rpx;">
															<mp-audio
																@click="playCall"
																ref="audioPlayers"
																:src="globalData.url+'static/audio/studycircle/'+item2"
																:poster="globalData.defaultHeadImage"
																:name="'音频'"
																:author="item.user.name"
																:controls="true"
															></mp-audio>
														</view>
														
													</template>
												
												</view>
												<view v-else style="justify-content: center;text-align: center;">
													暂无内容
												</view>
												</scroll-view>
											</uni-td>
											<uni-td align="center">
												<scroll-view :scroll-y="true" style="height: 210rpx;" >
													<view style="background-color: aliceblue;" v-if="globalData.Base64.decode(item.studyCircle.content,1)!='<p></p>'">
														<mp-html
															
															container-style="padding:20px"
															:domain="globalData.url + 'static'"
															:content="globalData.Base64.decode(item.studyCircle.content,1)"
															lazy-load
															scroll-table
															selectable
															use-anchor
															
														/>
													</view>
													<view v-else style="justify-content: center;text-align: center;">
														暂无内容
													</view>
												</scroll-view>
											</uni-td>
											<uni-td align="center">
												<view style="vertical-align: middle;">
													<view class="titles1" style="vertical-align: middle;">
														{{item.user.username}}
													</view>
													<view style="display: inline-block;vertical-align: middle;margin-left: 20rpx;">
														<image @error="basicStudyCircleUserImageError(index)" style="width:100rpx;height: 100rpx;border-radius: 50%;" :src="item.user.headimage"></image>
													</view>
													
												</view>			
											</uni-td>   
											<uni-td align="center">{{item.studyCircle.createtime.replace("T"," ")}}</uni-td>
										</uni-tr>
									</template>		
								</uni-table>
								<view>
									<uni-pagination  :total="studyCircleCondition.total" @change="clickStudyCirclePage"   :current="studyCircleCondition.current" :pageSize="studyCircleCondition.size"></uni-pagination>
								</view>
							</view>
						<!-- 学习圈管理view end -->
					</view>
					
				</view>
				</scroll-view>
			</view>
			
		</view>
		
		
		<view class="footer">
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				globalData:{},
				user:null,
				menu:11,
				
				//基本信息 start
				userBasicSetting:null,
				cikuBasicSetting:null,
				wordBasicSetting:null,
				readBasicSetting:null,
				testBasicSetting:null,
				scoreBasicSetting:null,
				productBasicSetting:null,
				orderBasicSetting:null,
				dataBasicSetting:null,
				studyCircleBasicSetting:null,
					//图表配置
				wordGraphOpts:{
					 dataLabel: false,
					 dataPointShape:false,
					 fontColor:'white',
					 xAxis:{
						 axisLineColor:"white",
						 gridColor:"white",
						  fontColor:'white',		 						
					 },
					 yAxis:{
						 axisLineColor:"white",
						 gridColor:"white",
						  fontColor:'white',
					 },
					
				},
				scoreGraphOpts:{
					 dataLabel: false,
					dataPointShape:false,
					 fontColor:'white',
					 xAxis:{
						 axisLineColor:"white",
						 gridColor:"white",
						 rotateLabel:true,
						  fontColor:'white',
						
					 },
					 yAxis:{
						 axisLineColor:"white",
						 gridColor:"white",
						  fontColor:'white',
					 },
					 extra:{
						 line:{
							 type:'curve'
						 }
					 }
				},
				basciOrderOpts:{
					fontColor:'white',
					legend:{
						fontColor:'white',
					}
				},
				//基本信息 end
				
				//首页设置 start
				indexSetting:null,
				indexSettingBackUp:null,
				rules:{
					everyDayWord:{
						label:'每日一词所展示的单词',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"word/getOneOpen",
													method:'POST',
													dataType:'json',
													header:{
														'content-type':this.globalData.contentType.normalData,
													},
													data:{
														word:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															resolve();
															this.setWordToEveryWordSetting(data.data[0]);
														}else{
															reject(new Error(data.message))
															
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}
			
			
									});
								
								}
							}
						]
					},
					baseEveryDayWordSize:{
						label:'每日一个单词的字体大小基本大小值',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 500;
									if (value==undefined||value==null||value==''||value == 0) {
										callback('{label}不能为空，请填写');
									} else if (value > maxLength || value < minLength) {
										callback('{label}应该在' +  minLength +"-" + maxLength + '之间');
									} else {
										callback();
									}
								}
							}
						]
					},
					baseEveryDayWordFactor:{
						label:'每日一个单词的基本长度',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 500;
									if (value==undefined||value==null||value==''||value == 0) {
										callback('{label}不能为空，请填写');
									} else if (value > maxLength || value < minLength) {
										callback('{label}应该在' +  minLength +"-" + maxLength + '之间');
									} else {
										callback();
									}
								}
							}
						]
					},
					baseEveryDayWordMutipiNum:{
						label:'每日一个单词的基本乘数系数',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 500;
									if (value==undefined||value==null||value==''||value == 0) {
										callback('{label}不能为空，请填写');
									} else if (value > maxLength || value < minLength) {
										callback('{label}应该在' +  minLength +"-" + maxLength + '之间');
									} else {
										callback();
									}
								}
							}
						]
					},
					signInText1:{
						label:'签到标语1',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 50;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					signInText2:{
						label:'签到标语2',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 50;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					learnText:{
						label:'学习标语',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 50;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					wordBankText:{
						label:'词库标语',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 50;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
					reviewText:{
						label:'复习标语',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction: function(rule, value, data, callback) {
									let minLength = 1;
									let maxLength = 50;
									if (value.trim().length == 0) {
										callback('{label}不能为空，请填写');
									} else if (value.trim().length > maxLength || value.trim().length < minLength) {
										callback('{label}长度应该在' +  minLength +"-" + maxLength + '位');
									} else {
										callback();
									}
								}
							}
						]
					},
				},
				
				//首页设置 end
				
				//用户管理 start
				userModuleMenuTexts:[],
				userModuleMenu:0,
				users:[],
				chooseUsersIndex:[],
				userSexFilterData:[{text:'男',value:0},{text:'女',value:1},{text:'保密',value:2}],
				userCondition:{
					usernameSearch:null,
					passwordSearch:null,
					nameSearch:null,
					sexSelect:null,
					birthdayDate:null,
					orderbyDesc:[],
					orderbyAsc:[],
					phoneSearch:null,
					emailSearch:null,
					current:1,
					size:6,
					sizeBackUp:6,
					total:0,
				},
				//用户管理 end
				
				//词库管理 start
				cikuModuleMenuTexts:[],
				cikuModuleMenu:0,
				cikus:[],
				cikuUserFilterData:[],
				chooseCikusIndex:[],
				cikutypeModal:"insert",
				cikutypeEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				cikutypeEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				cikuTypeRules:{
					dsc:{
						label:'词库类别名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminCiku/getCikuTypeExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				cikuCondition:{
					dscSearch:null,
					dscabbSearch:null,
					userSelect:null,		
					orderbyDesc:[],
					orderbyAsc:[],
					cikuTypeId:0,
					current:1,
					size:6,
					sizeBackUp:6,
					total:0,
				},
				//词库管理 end
				
				//单词管理 start
				wordModuleMenuTexts:[],
				wordModuleMenu:0,
				words:[],
				chooseWordsIndex:[],
				wordtypeModal:"insert",
				wordCondition:{
					initial:null,
					wordSearch:null,
					transSearch:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				//单词管理 end
				
				//阅读管理 start
				readModuleMenuTexts:[],
				readModuleMenu:0,
				readSubModuleMenuTexts:[],
				readSubModuleMenu:0,
				reads:[],
				chooseReadsIndex:[],
				readtypeModal:"insert",
				readCondition:{
					readType:null,
					readTypeSub:null,
					essaySearch:null,
					nameSearch:null,
					authorSearch:null,
					briefSearch:null,
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				readtypeModal:"insert",
				readtypeEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				readtypeEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				readTypeRules:{
					dsc:{
						label:'阅读一级类别名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminRead/getReadTypeExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				readtypeSubModal:"insert",
				readtypeSubEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				readtypeSubEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				readTypeSubRules:{
					dsc:{
						label:'阅读二级类别名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminRead/getReadTypeSubExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value,
														readType:this.readModuleMenuTexts[this.readModuleMenu].id,
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				//阅读管理  end
				
				//题库管理 start
				testModuleMenuTexts:[],
				testModuleMenu:0,
				tests:[],
				chooseTestsIndex:[],
				testtypeModal:"insert",
				testAnswerFilterData:[{text:'A',value:0},{text:'B',value:1},{text:'C',value:2},{text:'D',value:3}],
				testCondition:{
					testType:null,
					
					titleSearch:null,
					optaSearch:null,
					optbSearch:null,
					optcSearch:null,
					optdSearch:null,
					answerSelect:null,
					analySearch:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				testtypeEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				testtypeEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				testTypeRules:{
					dsc:{
						label:'题库名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminTest/getTestTypeExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				//题库管理 end
				
				//积分管理 start
				scoreModuleMenuTexts:['积分来源类别管理','积分管理','流水管理'],
				scoreModuleMenu:0,
				scoreSources:[],
				chooseScoreSourcesIndex:[],
				
				scoreSourceTypeFilterData:[{text:'加+',value:0},{text:'减-',value:1}],
				scoreSourceCondition:{
					
					dscSearch:null,

					typeSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:10,
					sizeBackUp:10,
					total:0,
				},
				
				
				
				scores:[],
				scoreUserFilterData:[],
				scoreCondition:{
					userSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:7,
					sizeBackUp:7,
					total:0,
				},
				scoreEntity:{
					id:null,
					uid:null,
					score:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				scoreRules:{
					score:{
						label:'积分数值',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'number',
								errorMessage: '{label}只为数值'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 0;
										let maxLength = 99999999;
										if (value==null) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value> maxLength || value < minLength) {
											reject(new Error('{label}应该在'+  minLength +"-" + maxLength + '之间'));
										} else {
											resolve();
										}		
									});
								}
							}
						]
					},
				},
				
				zborders:[],
				zbOrderUserFilterData:[],
				zbOrderStatusFilterData:[{text:'未支付',value:0},{text:'已支付',value:1},{text:'已取消',value:2}],
				zbOrderCondition:{
					zbcodeSearch:null,
					zborderidSearch:null,
					userSelect:null,
					statusSelect:null,
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:5,
					sizeBackUp:5,
					total:0,
				},
				zbOrderEntity:{
					zborder:{
						id:null,
						uid:null,
						zbcode:null,
						money:null,
						zborderid:null,
						dsc:null,
						dscabb:null,
						status:null,
						deleted:0,
						createtime:null,
						updatetime:null,
					},
					user:{}
					
				},
				zbOrderEntityBackUp:{
					zborder:{
						id:null,
						uid:null,
						zbcode:null,
						money:null,
						zborderid:null,
						dsc:null,
						dscabb:null,
						status:null,
						deleted:0,
						createtime:null,
						updatetime:null,
					},
					user:{}
				},
				zbOrderRules:{
					money:{
						label:'金额',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'number',
								errorMessage: '{label}只为数值'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 0;
										let maxLength = 10000;
										if (value==null) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value> maxLength || value < minLength) {
											reject(new Error('{label}应该在'+  minLength +"-" + maxLength + '之间'));
										} else {
											resolve();
										}		
									});
								}
							}
						]
					},
				},
				//积分管理 end
				
				//商品管理 start
				productModuleMenuTexts:[],
				productModuleMenu:0,
				productSubModuleMenuTexts:[],
				productSubModuleMenu:0,
				productMerchantFilterData:[],
				products:[],
				chooseProductsIndex:[],
				ptypeModal:"insert",
				productCondition:{
					pType:0,
					pTypeSub:0,
					titleSearch:null,
					merchantSelect:null,
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				ptypeModal:"insert",
				ptypeEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				ptypeEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				pTypeRules:{
					dsc:{
						label:'商品一级类别名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminProduct/getPTypeExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				ptypeSubModal:"insert",
				ptypeSubEntity:{
					id:null,
					ptypeid:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				ptypeSubEntityBackUp:{
					id:null,
					ptypeid:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				pTypeSubRules:{
					dsc:{
						label:'商品二级类别名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminProduct/getPTypeSubExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value,
														pType:this.productModuleMenuTexts[this.productModuleMenu].id,
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				//商品管理 end
				
				
				//订单管理 start
				orderModuleMenuTexts:[],
				orderModuleMenu:0,
				orders:[],
				ordersNums:[],
				chooseOrdersIndex:[],
				orderstatusModal:"insert",
				orderProductFilterData:[],
				
				orderUserFilterData:[],
				orderCondition:{
					orderstatus:null,
					
					ordersSearch:null,
					
					productSelect:null,
					
					userSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				orderConditionBackUp:{
					orderstatus:null,
					
					ordersSearch:null,
					
					productSelect:null,
					addressSelect:null,
					userSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:3,
					sizeBackUp:3,
					total:0,
				},
				orderstatusEntity:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				orderstatusEntityBackUp:{
					id:null,
					dsc:null,
					deleted:0,
					createtime:null,
					updatetime:null,
				},
				orderStatusRules:{
					dsc:{
						label:'订单类型名称',
						rules:[
							{
								require:true,
								errorMessage: '{label}不能为空，请填写',
							},
							{
								format: 'string',
								errorMessage: '{label}只为字符串'
							},
							{
								validateFunction:(rule, value, data, callback) => {
									// 异步需要返回 Promise 对象
									return new Promise((resolve, reject) => {
										let minLength = 1;
										let maxLength = 50;
										if (value.toString().length == 0) {
											reject(new Error('{label}不能为空，请填写'))
										} else if (value.toString().length > maxLength || value.toString().length < minLength) {
											reject(new Error('{label}长度应该在'+  minLength +"-" + maxLength + '位'));
										} else {
											uni.request({
													url:this.globalData.url+"adminOrder/getOrderStatusExist",
													method:'POST',
													dataType:'json',
													header:this.globalData.header.normal,
													data:{
														dsc:value
													},
													success: (res) => {
														let data =res.data;
														if(data.code==200){
															reject(new Error(data.message))
														}else{
															resolve();
														}
													},
													fail: (error) => {
														reject(new Error('请求数据错误'))
													}
											});
										}		
									});
								}
							}
						]
					},
				},
				//订单管理 end
				
				//数据管理 start
				allDatas:[],
				//数据管理 end
				
				//学习圈管理 start
				studycircles:[],
				chooseStudyCirclesIndex:[],
				studyCircleTagsFilterData:[],
				studyCircleUserFilterData:[],
				studyCircleCondition:{
					
					titleSearch:null,
					tagsSelect:null,
					
					userSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:4,
					sizeBackUp:4,
					total:0,
				},
				studyCircleConditionBackUp:{
					titleSearch:null,
					tagsSelect:null,
					
					userSelect:null,
					
					orderbyDesc:[],
					orderbyAsc:[],
					
					current:1,
					size:4,
					sizeBackUp:4,
					total:0,
				},
				//学习圈管理 end
			}
		},
		onLoad(param) {
			this.globalData=getApp().globalData;
			let menu =   uni.getStorageSync("menu");
			if(menu!=''){
					this.menu=parseInt(menu);
					uni.removeStorageSync("menu");
			}
		},
		onShow() {
			this.globalData=getApp().globalData;
			
			if(this.globalData.checkLogin()){
				this.user = JSON.parse(uni.getStorageSync("user"));
				console.log(this.user);
				this.user.headimage = this.globalData.url+"static/Image/headImage/"+this.user.headimage;
				this.loadData();
			}
		},
		onReady() {
		
			if(this.menu==1){
				this.$refs['indexSetting'].setRules(this.rules);
			}
			// if(this.menu==3){
			// 	this.$refs['cikutypeEntity'].setRules(this.cikuTypeRules);
			// }
		},
		
		methods: {
			//单击菜单 start
			clickMenu(index){
				this.menu =index;
				this.initMenu(index);
			},
			//单击菜单 end
			//初始化菜单 start
			initMenu(index){
				switch(index){
					//基本信息start
					case 0:{
						this.loadUserBasicSetting().then(()=>{
							this.loadBasicCikuSetting().then(()=>{
								this.loadBasicWordSetting().then(()=>{
									this.loadBasicReadSetting().then(()=>{
										this.loadBasicTestSetting().then(()=>{
											this.loadBasicScoreSetting().then(()=>{
												this.loadBasicProductSetting().then(()=>{
													this.loadBasicOrderSetting().then(()=>{
														this.loadBasicDataSetting().then(()=>{
															this.loadBasicStudyCircleSetting().then(()=>{
																
															})
														})
													})
												})
											})
										})
									})
								});
							});
						});
						break;
					}
					//基本信息end
					case 1:{
						this.loadMainSetting();
						break;
					}
					case 2:{
						this.userModuleMenuTexts=[];
						this.userModuleMenuTexts.push("用户管理");
						if(JSON.parse(uni.getStorageSync("user")).type==-1){
							this.userModuleMenuTexts.push("管理员管理");
						}
						this.users=[];
						this.userCondition.current=1,
						this.userCondition.size=this.userCondition.sizeBackUp,
						this.userCondition.total=0,
						this.loadUsers();
						break;
					}
					case 3:{
						this.cikuModuleMenuTexts=[];
						this.loadCikuTypes().then(()=>{
							this.cikus=[];
							this.cikuCondition.current=1,
							this.cikuCondition.size=this.cikuCondition.sizeBackUp,
							this.cikuCondition.total=0,
							this.loadCikus().then(()=>{
								this.loadAdmin();
							});
						})

						break;
					}
					case 4:{
						this.wordModuleMenuTexts=[];
						this.wordCondition.initial="a";
						this.loadWordInitials().then(()=>{
							
							this.loadWords().then(()=>{
								
							})
						});
						break;
					}
					case 5:{
						this.readModuleMenuTexts=[];
						this.readSubModuleMenuTexts=[];
						this.readModuleMenu=0;
						this.readSubModuleMenu=0;
						this.readCondition.readType=1;
						this.readCondition.readTypeSub=1;
						this.loadReadType().then(()=>{
							this.loadReadTypeSub().then(()=>{
								this.loadReads().then(()=>{
									
								})
							})
						});
						break;
					}
					case 6:{
						this.testModuleMenuTexts=[];
						this.testModuleMenu=0;
						this.testCondition.testType=1;
						this.loadTestType().then(()=>{
							this.loadTests().then(()=>{
								
							})
							
						});
						
						break;
					}
					case 7:{
						if(uni.getStorageSync("scoreIndex")!=''){
							this.scoreModuleMenu=parseInt(uni.getStorageSync("scoreIndex"));
							uni.removeStorageSync("scoreIndex");
						}
						switch (this.scoreModuleMenu){
							case 0:{
								this.loadScoreSources();
								break;
							}
							case 1:{
								this.loadScores().then(()=>{
									this.loadAllUsers();
								})
								break;
							}
							case 2:{
								this.loadZbOrders().then(()=>{
									this.loadAllUsers();
								})
								break;
							}
							default: {
								
								break;
							}
						}
						break;
					}
					case 8:{
						this.productModuleMenuTexts=[];
						this.productSubModuleMenuTexts=[];
						this.productModuleMenu=0;
						this.productSubModuleMenu=0;
						this.productCondition.pType=1;
						
						
						
						this.loadPType().then(()=>{
							if(uni.getStorageSync("pType")!=''){
								this.productCondition.pType=parseInt(uni.getStorageSync("pType"));
								this.productModuleMenuTexts.forEach((item,index)=>{
									if(item.id==this.productCondition.pType){
										this.productModuleMenu=index;
									}
								})
								uni.removeStorageSync("pType");
							}
							this.loadPTypeSub().then(()=>{
								if(uni.getStorageSync("pTypeSub")!=''){
									this.productCondition.pTypeSub=parseInt(uni.getStorageSync("pTypeSub"));
									this.productSubModuleMenuTexts.forEach((item,index)=>{
										if(item.id==this.productCondition.pTypeSub){
											this.productSubModuleMenu=index;
										}
									})
									uni.removeStorageSync("pTypeSub");
								}
								if(uni.getStorageSync("productCurrent")!=''){
									this.productCondition.current=parseInt(uni.getStorageSync("productCurrent"));
									uni.removeStorageSync("productCurrent");
								}
								this.loadProducts().then(()=>{
									this.loadAllMerchants();
								})
							})
						});
						break;
					}
					case 9:{
						this.orderModuleMenuTexts=[];
						this.orderModuleMenu=0;
						this.orderCondition.orderstatus=1;
						this.loadOrderStatus().then(()=>{
							this.loadOrderNums().then(()=>{
								this.loadOrders().then(()=>{
									this.loadOrderProducts().then(()=>{
										this.loadAllUsers();
									});
								})
							})
							
							
						});
						break;
					}
					case 10:{
						this.loadAllDatas();
						break;
					}
					case 11:{
						this.loadStudyCircles().then(()=>{
							this.loadTags();
						});
						break;
					}
					default:{
						
						break;
					}
				}
			},
			//初始化菜单 end
			
			//图片错误 start
			//学习圈管理 start
			basicStudyCircleUserImageError(index){
				this.studycircles[index].user.headimage=this.globalData.defaultHeadImage;
			},
			//学习圈管理 end
			//订单管理 start
			basicOrdersProductImageError(index){
				this.orders[index].product.product.product.imgAddre=this.globalData.defaultHeadImage;
			},
			basicOrdersProductUserImageError(index){
				this.orders[index].product.product.user.headimage=this.globalData.defaultHeadImage;
			},
			basicOrdersUserImageError(index){
				this.orders[index].user.headimage=this.globalData.defaultHeadImage;
			},
			//订单管理 end
			
			//商品管理 start
			basicProductsImageError(index){
				this.products[index].product.img_addre = this.globalData.defaultHeadImage;
			},
			basicProductsUserImageError(index){
				this.products[index].user.headimage = this.globalData.defaultHeadImage;
			},
			//商品管理 end
			
			//积分管理用户头像错误 start
			basicZbordersPopImageError(){
				this.zbOrderEntity.user.headimage = this.globalData.defaultHeadImage;
			},
			basicZbordersImageError(index){
				this.zborders[index].user.headimage = this.globalData.defaultHeadImage;
			},
			basicScoresImageError(index){
				this.scores[index].user.headimage = this.globalData.defaultHeadImage;
			},
			//积分管理用户头像错误 end
			//阅读管理封面错误 start
			basicReadsImageError(index){
				this.reads[index].image = this.globalData.defaultHeadImage;
			},
			//阅读管理封面错误 end
			//词库管理 创建者用户头像错误 start
			basicCikusImageError(index){
				this.cikus[index].user.headimage = this.globalData.defaultHeadImage;
			},
			//词库管理 创建者用户头像错误 end
			//用户管理 用户头像错误 start
			basicUsersImageError(index){
				this.users[index].headimage = this.globalData.defaultHeadImage;
			},
			//用户管理 用户头像错误 start
			//基本信息 用户管理 图片错误 start
			basicUserSettingImageError(index){
				this.userBasicSetting.lastRegisters[index].headimage = this.globalData.defaultHeadImage;
			},
			//基本信息 用户管理 图片错误 end
			imageError(){
				this.user.headimage = this.globalData.defaultHeadImage;
			},
			//基本信息 积分管理 用户头像图片错误 start
			basicScoreSettingImageError(){
				this.scoreBasicSetting.max.user.headimage = this.globalData.defaultHeadImage;
			},
			//基本信息 商品管理 用户头像图片，商品图片错误 start
			basicProductSettingImageError(index,type){
				if(type==0){
					this.productBasicSetting.nodes[index].products.imgAddre =  this.globalData.defaultHeadImage;
				}else{
					this.productBasicSetting.nodes[index].merchant.headimage= this.globalData.defaultHeadImage;
				}
			
			},
			//基本信息 学习圈管理 用户头像图片错误 start
			basicStudyCircleSettingImageError(index){
				this.studyCircleBasicSetting.lastStudyCircles[index].user.headimage = this.globalData.defaultHeadImage;
			},
			//图片错误 end
			
			//杂项 start
				//预览图片  start
				previewImage(url){
					uni.previewImage({
						current:0,
						urls:[url],
						longPressActions: {
							itemList: ['发送给朋友', '保存图片', '收藏'],
							success: function(data) {
								console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
							},
							fail: function(err) {
								console.log(err.errMsg);
							}
						}
					});
				},
				//预览图片 end
				//播放读音 start
				//播放中文 start
				playChinese(e){
					if (e != undefined && e != 'undefined') {
						
						let url =`${this.globalData.audioSentenceUrl}type=2&audio=${encodeURIComponent(e)}&le=zh`;
						this.playWordSound(url);
					}
				},
				//播放中文 end
				//播放英音句子 start
				playUKSentence(e) {
					if (e != undefined && e != 'undefined') {
						let url = this.globalData.audioSentenceUrl + 'type=1&' + 'audio=' + e;
						console.log(url);
						this.playWordSound(url);
					}
				},
				//播放英音句子 end
				//播放美音句子 start
				playUSASentence(e) {
					if (e != undefined && e != 'undefined') {
						let url = this.globalData.audioSentenceUrl + 'type=2&' + 'audio=' + e;
						console.log(url);
						this.playWordSound(url);
					}
				},
				//播放美音句子 end
				//播放英音 start
				playUK(e) {
					let url = `${this.globalData.mediaUrl}audio/1/${e.slice(0,1).toLowerCase()}/${e}.mp3`;
					this.playWordSound(url);
				},
				//播放英音 end
				//播放美音 start
				playUSA(e) {
					let url = `${this.globalData.mediaUrl}audio/2/${e.slice(0,1).toLowerCase()}/${e}.mp3`;
					this.playWordSound(url);
				},
				//播放美音 end
				//播放读音  start
				playWordSound(url) {
					console.log(url);
					if (!this.isPlayingWord) {
						const innerAudioContext = uni.createInnerAudioContext();
						innerAudioContext.autoplay = true;
						innerAudioContext.src = url;
						innerAudioContext.onPlay(() => {
							this.isPlayingWord = true;
							console.log('开始播放');
						});
						innerAudioContext.onEnded(() => {
							this.isPlayingWord = false;
						});
						innerAudioContext.onError(res => {
							console.log(res.errMsg);
							console.log(res.errCode);
						});
					}
				},
				//播放读音  end
				//播放读音 end
				
				//首页设置 start
					//将获得的单词放进每日一词setting 中 start
					setWordToEveryWordSetting(word){		
							this.indexSetting.data.everyDayWordModule.everyDayWord.value=word.word;
							this.indexSetting.data.everyDayWordModule.everyDayWord.trans=word.trans;
							this.indexSetting.data.everyDayWordModule.everyDayWord.soundmark1=word.soundmark1;
							this.indexSetting.data.everyDayWordModule.everyDayWord.soundmark2=word.soundmark2;
							this.indexSetting.data.everyDayWordModule.everyDayWord.soundUrl1=`audio/1/${word.word.slice(0, 1).toLowerCase()}/${word.word}.mp3`;
							this.indexSetting.data.everyDayWordModule.everyDayWord.soundUrl2=`audio/2/${word.word.slice(0, 1).toLowerCase()}/${word.word}.mp3`;
					},	
					//将获得的单词放进每日一词setting 中 end
				//首页设置 end
			//杂项 end
			
			//选择图片 start
				//首页设置 start
					//选择背景图片 start
					backgroundSelect(res){
						console.log(res.tempFilePaths[0]);
						uni.uploadFile({
							url:this.globalData.url+'adminFile/uploadImageBackGround',
							filePath:res.tempFilePaths[0],
							name:'file',
							header:{
								'token':uni.getStorageSync('token')
							},
							xhrFields: { withCredentials: true },
							
						}).then((res) => {
						    // 此处的 res 参数，与使用默认方式调用时 success 回调中的 res 参数一致
						    // console.log(res.data);
							uni.hideLoading();
							let data =JSON.parse(res.data);
							if(data.code==200){
								this.indexSetting.bgUrl = data.data;
							}else{
								
								uni.showToast({
									title:data.message,
									icon:'error'
								})
							}
							// this.resetFormData();
						  })
						  .catch((err) => {
						    // 此处的 err 参数，与使用默认方式调用时 fail 回调中的 err 参数一致
							
						    // console.log(err);
							uni.showToast({
								title:"提交错误",
								icon:'error'
							});
							// this.resetFormData();
						  });
					},
					//选择背景图片 end
				//首页设置 end
			//选择图片 end
			
			
			
			//移除选择图片 start
				//首页设置 start
					//移除选择背景图片 start
					backgroundDelete(e) {
						console.log('删除文件从列表', e);
						this.indexSetting.bgUrl="";
					},
					//移除选择背景图片 end
				//首页设置 end
			//移除选择图片 end
		
			//二级menu点击事件 start
				//用户管理 start
					onClickUserModuleMenuItem(index){
						this.userModuleMenu = index;
						this.userModuleMenuTexts=[];
						this.userModuleMenuTexts.push("用户管理");
						if(JSON.parse(uni.getStorageSync("user")).type==-1){
							this.userModuleMenuTexts.push("管理员管理");
						}
						this.users=[];
						this.userCondition.current=1;
						this.userCondition.size=this.userCondition.sizeBackUp;
						this.userCondition.total=0;
						this.loadUsers();
					},
				//用户管理 end
				//词库管理 start
					onClickCikuModuleMenuItem(index){
						this.cikuModuleMenu = index;
						this.cikuCondition.cikuTypeId = this.cikuModuleMenuTexts[index].id;
						this.cikus=[];
						this.cikuCondition.current=1;
						this.cikuCondition.size=this.cikuCondition.sizeBackUp;
						this.cikuCondition.total=0;
						this.loadCikus().then(()=>{
							this.loadAdmin();
						});
					},
				//词库管理 end
				//单词管理 start
					onClickWordModuleMenuItem(index){
						this.wordModuleMenu = index;
						this.wordCondition.initial=this.wordModuleMenuTexts[index].text;
						this.words=[];
						this.wordCondition.current=1;
						this.wordCondition.size=this.wordCondition.sizeBackUp;
						this.wordCondition.total=0;
						this.loadWords().then(()=>{
							
						});
					},
				//单词管理 end
				//阅读管理 start
					onClickReadModuleMenuItem(index){
						this.readModuleMenu = index;
						this.readCondition.readType=this.readModuleMenuTexts[index].id;
						this.reads=[];
						this.readSubModuleMenuTexts=[];
						this.readSubModuleMenu=0;
						this.readCondition.readTypeSub=1;
						this.readCondition.current=1;
						this.readCondition.size=this.readCondition.sizeBackUp;
						this.readCondition.total=0;
						
						this.loadReadTypeSub().then(()=>{
							this.loadReads().then(()=>{
								
							})
						})
						
					},
					onClickReadSubModuleMenuItem(index){
						this.readSubModuleMenu = index;
						this.readCondition.readTypeSub=this.readSubModuleMenuTexts[index].id;
						this.reads=[];
						this.readCondition.current=1;
						this.readCondition.size=this.readCondition.sizeBackUp;
						this.readCondition.total=0;
						this.loadReads().then(()=>{
							
						});
					},
				//阅读管理 end
				//题库管理 start
					onClickTestModuleMenuItem(index){
						this.testModuleMenu = index;
						this.testCondition.testType=this.testModuleMenuTexts[index].id;
						this.tests=[];

						this.testCondition.current=1;
						this.testCondition.size=this.testCondition.sizeBackUp;
						this.testCondition.total=0;
						
						this.loadTestType().then(()=>{
							this.loadTests().then(()=>{
								
							})
						})
						
					},

				//题库管理 end
				//积分管理 start
					onClickScoreModuleMenuItem(index){
						this.scoreModuleMenu = index;
						
						// this.scoreSources=[];
					
						// this.scoreSourceCondition.current=1;
						// this.scoreSourceCondition.size=this.scoreSourceCondition.sizeBackUp;
						// this.scoreSourceCondition.total=0;
						switch (this.scoreModuleMenu){
							case 0:{
								this.loadScoreSources();
								break;
							}
							case 1:{
								this.loadScores().then(()=>{
									this.loadAllUsers();
								})
								break;
							}
							case 2:{
								this.loadZbOrders().then(()=>{
									this.loadAllUsers();
								})
								break;
							}
							default: {
								
								break;
							}
						}
						
						
					},
				//积分管理 end
				//商品管理 start
					onClickProductModuleMenuItem(index){
						this.productModuleMenu = index;
						this.productCondition.pType=this.productModuleMenuTexts[index].id;
						this.products=[];
						this.productSubModuleMenuTexts=[];
						this.productSubModuleMenu=0;
						this.productCondition.current=1;
						this.productCondition.size=this.productCondition.sizeBackUp;
						this.productCondition.total=0;
						
						this.loadPTypeSub().then(()=>{
							this.loadProducts().then(()=>{
								this.loadAllMerchants();
							})
						})
						
					},
					onClickProductSubModuleMenuItem(index){
						this.productSubModuleMenu = index;
						this.productCondition.pTypeSub=this.productSubModuleMenuTexts[index].id;
						this.products=[];
						this.productCondition.current=1;
						this.productCondition.size=this.productCondition.sizeBackUp;
						this.productCondition.total=0;
						this.loadProducts().then(()=>{
							this.loadAllMerchants();
						});
					},
				//商品管理 end
				//订单管理 start
					onClickOrderModuleMenuItem(index){
						this.orderModuleMenu = index;
						this.orderCondition = JSON.parse(JSON.stringify(this.orderConditionBackUp));
						this.orderCondition.orderstatus=this.orderModuleMenuTexts[index].id;
						this.orders=[];
						this.chooseOrdersIndex=[];
						this.orderCondition.current=1;
						this.orderCondition.size=this.orderCondition.sizeBackUp;
						this.orderCondition.total=0;
						
						this.loadOrderStatus().then(()=>{
							this.loadOrderNums().then(()=>{
								this.loadOrders().then(()=>{
									this.loadOrderProducts().then(()=>{
										this.loadAllUsers();
									});
								})
							})
						})
						
					},
				//订单管理 end
			//二级menu点击事件 end
		
		
		
			//页码按钮 start
				//用户管理 start
				clickUserPage(e){
					this.userCondition.current=e.current;
					this.loadUsers();
				},
				//用户管理 end
				//词库管理 start
				clickCikuPage(e){
					this.cikuCondition.current=e.current;
					this.loadCikus();
				},
				//词库管理 end
				//单词管理 start
				clickWordPage(e){
					this.wordCondition.current=e.current;
					this.loadWords();
				},
				//单词管理 end
				//阅读管理 start
				clickReadPage(e){
					this.readCondition.current=e.current;
					this.loadReads();
				},
				//阅读管理 end
				//题库管理 start
				clickTestPage(e){
					this.testCondition.current=e.current;
					this.loadTests();
				},
				//题库管理 end
				//积分管理 start
				clickScoreSourcePage(e){
					this.scoreSourceCondition.current=e.current;
					this.loadScoreSources();
				},
				clickScorePage(e){
					this.scoreCondition.current=e.current;
					this.loadScores();
				},
				clickZbOrderPage(e){
					this.zbOrderCondition.current=e.current;
					this.loadZbOrders();
				},
				//积分管理 end
				//商品管理 start
				clickProductPage(e){
					this.productCondition.current=e.current;
					this.loadProducts();
				},
				//商品管理 end
				//订单管理 start
				clickOrderPage(e){
					this.orderCondition.current=e.current;
					this.loadOrders();
				},
				//订单管理 end
				//学习圈管理 start
				clickStudyCirclePage(e){
					this.studyCircleCondition.current=e.current;
					this.loadStudyCircles();
				},
				//学习圈管理 end
			//页码按钮 end
			
		
			//actionSheet start
				//main start
				showActionBarByMain(){
					uni.showActionSheet({
						itemList: ['修改个人信息','退出登录'],
						success:(res)=> {
							if(res.tapIndex==0){
								//修改个人信息
								uni.navigateTo({
									url:"/pages/userInfo/userInfo"
								})
							}else{
								//退出登录
								uni.showModal({
									title:'注意',
									content:'您确定要退出登录吗？',
									success: (res) => {
										if(res.confirm){
											uni.clearStorage();
											
											
											uni.navigateTo({
												url:"/pages/login/login"
											})
										}
									}
								})
							}
						},
						fail: function (res) {
							console.log(res.errMsg);
						}
					});
				},
				//main end
				
			//actionSheet end
		
			//表格table 多选框change start
				//学习圈管理表格 start
				studyCircleTableSelectChange(obj){
					this.chooseStudyCirclesIndex=obj.detail.index;
					
				},
				//学习圈管理表格 end
				//订单管理表格 start
				orderTableSelectChange(obj){
					this.chooseOrdersIndex=obj.detail.index;
					
				},
				//订单管理表格 end
				//商品管理表格 start
				productTableSelectChange(obj){
					this.chooseProductsIndex=obj.detail.index;
					
				},
				//商品管理表格 end
				//积分管理表格 start
				scoreSourceTableSelectChange(obj){
					this.chooseScoreSourcesIndex=obj.detail.index;
					
				},
				//积分管理表格 end
				//题库管理表格 start
				testTableSelectChange(obj){
					this.chooseTestsIndex=obj.detail.index;
					
				},
				//题库管理表格 end
				
				//阅读管理表格 start
				readTableSelectChange(obj){
					this.chooseReadsIndex=obj.detail.index;
					
				},
				//阅读管理表格 end
				
				//单词管理表格 start
				wordTableSelectChange(obj){
					this.chooseWordsIndex=obj.detail.index;
					
				},
				//单词管理表格 end
				//词库管理表格 start
				cikuTableSelectChange(obj){
					this.chooseCikusIndex=obj.detail.index;
					
				},
				//词库管理表格 end
				//用户管理表格 start
				userTableSelectChange(obj){
					this.chooseUsersIndex=obj.detail.index;
					
				},
				//用户管理表格 end
			//表格table 多选框change end
		
			//表格table 排序 start
				//学习圈管理 start
					//创建时间排序 start
					studyCircleTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.studyCircleCondition.orderbyAsc.length; i++) {
								if(this.studyCircleCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.studyCircleCondition.orderbyDesc.length; i++) {
								if(this.studyCircleCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.studyCircleCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.studyCircleCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.studyCircleCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.studyCircleCondition.orderbyDesc.push(conlumn);
						}
						this.loadStudyCircles();
					},
					//创建时间排序 end
				//学习圈管理 end
				//订单管理 start
					//创建时间排序 start
					orderTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.orderCondition.orderbyAsc.length; i++) {
								if(this.orderCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.orderCondition.orderbyDesc.length; i++) {
								if(this.orderCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.orderCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.orderCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.orderCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.orderCondition.orderbyDesc.push(conlumn);
						}
						this.loadOrders();
					},
					//创建时间排序 end
				//订单管理 end
				//商品管理 start
					//库存排序 start
					productNumSort(obj){
						let conlumn ="num";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.productCondition.orderbyAsc.length; i++) {
								if(this.productCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.productCondition.orderbyDesc.length; i++) {
								if(this.productCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.productCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.productCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.productCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.productCondition.orderbyDesc.push(conlumn)
						}
						this.loadProducts();
					},
					//库存排序 end
					//价格排序 start
					productPriceSort(obj){
						let conlumn ="price";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.productCondition.orderbyAsc.length; i++) {
								if(this.productCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.productCondition.orderbyDesc.length; i++) {
								if(this.productCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.productCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.productCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.productCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.productCondition.orderbyDesc.push(conlumn)
						}
						this.loadProducts();
					},
					//价格排序 end
					//创建时间排序 start
					productTimeSort(obj){
						let conlumn ="time";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.productCondition.orderbyAsc.length; i++) {
								if(this.productCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.productCondition.orderbyDesc.length; i++) {
								if(this.productCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.productCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.productCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.productCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.productCondition.orderbyDesc.push(conlumn)
						}
						this.loadProducts();
					},
					//创建时间排序 end
				//商品管理 end
				//积分管理 start
					//充值订单创建时间排序 start
					zbOrderTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.zbOrderCondition.orderbyAsc.length; i++) {
								if(this.zbOrderCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.zbOrderCondition.orderbyDesc.length; i++) {
								if(this.zbOrderCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.zbOrderCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.zbOrderCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.zbOrderCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.zbOrderCondition.orderbyDesc.push(conlumn)
						}
						this.loadZbOrders();
					},
					//充值订单创建时间排序 end
					//充值金额排序 start
					zbOrderMoneySort(obj){
						let conlumn ="money";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.zbOrderCondition.orderbyAsc.length; i++) {
								if(this.zbOrderCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.zbOrderCondition.orderbyDesc.length; i++) {
								if(this.zbOrderCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.zbOrderCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.zbOrderCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.zbOrderCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.zbOrderCondition.orderbyDesc.push(conlumn)
						}
						this.loadZbOrders();
					},
					//充值金额排序 end
					//充值积分排序 start
					zbOrderDscSort(obj){
						let conlumn ="dsc";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.zbOrderCondition.orderbyAsc.length; i++) {
								if(this.zbOrderCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.zbOrderCondition.orderbyDesc.length; i++) {
								if(this.zbOrderCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.zbOrderCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.zbOrderCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.zbOrderCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.zbOrderCondition.orderbyDesc.push(conlumn)
						}
						this.loadZbOrders();
					},
					//充值积分排序 end
					//积分排序 start
					scoreScoreSort(obj){
						let conlumn ="score";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.scoreCondition.orderbyAsc.length; i++) {
								if(this.scoreCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.scoreCondition.orderbyDesc.length; i++) {
								if(this.scoreCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.scoreCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.scoreCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.scoreCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.scoreCondition.orderbyDesc.push(conlumn)
						}
						this.loadScores();
					},
					//积分排序 end
					//积分创建时间排序 start
					scoreTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.scoreCondition.orderbyAsc.length; i++) {
								if(this.scoreCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.scoreCondition.orderbyDesc.length; i++) {
								if(this.scoreCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.scoreCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.scoreCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.scoreCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.scoreCondition.orderbyDesc.push(conlumn)
						}
						this.loadScores();
					},
					//积分创建时间排序 end
					//默认值排序 start
					scoreSourceDefaultsSort(obj){
						let conlumn ="defaults";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.scoreSourceCondition.orderbyAsc.length; i++) {
								if(this.scoreSourceCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.scoreSourceCondition.orderbyDesc.length; i++) {
								if(this.scoreSourceCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.scoreSourceCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.scoreSourceCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.scoreSourceCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.scoreSourceCondition.orderbyDesc.push(conlumn)
						}
						this.loadScoreSources();
					},
					//默认值排序 end
					//积分来源创建时间排序 start
					scoreSourceTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.scoreSourceCondition.orderbyAsc.length; i++) {
								if(this.scoreSourceCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.scoreSourceCondition.orderbyDesc.length; i++) {
								if(this.scoreSourceCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.scoreSourceCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.scoreSourceCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.scoreSourceCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.scoreSourceCondition.orderbyDesc.push(conlumn)
						}
						this.loadScoreSources();
					},
					//积分来源创建时间排序 end
				//积分管理 end
				//题库管理 start
					//创建时间排序 start
					testTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.testCondition.orderbyAsc.length; i++) {
								if(this.testCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.testCondition.orderbyDesc.length; i++) {
								if(this.testCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.testCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.testCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.testCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.testCondition.orderbyDesc.push(conlumn)
						}
						this.loadTests();
					},
					//创建时间排序 end
				//题库管理 end
				//阅读管理 start
					//创建时间排序 start
					readTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.readCondition.orderbyAsc.length; i++) {
								if(this.readCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.readCondition.orderbyDesc.length; i++) {
								if(this.readCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.readCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.readCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.readCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.readCondition.orderbyDesc.push(conlumn)
						}
						this.loadReads();
					},
					//创建时间排序 end
				//阅读管理 end
				//单词管理 start
					//创建时间排序 start
					wordTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.wordCondition.orderbyAsc.length; i++) {
								if(this.wordCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.wordCondition.orderbyDesc.length; i++) {
								if(this.wordCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.wordCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.wordCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.wordCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.wordCondition.orderbyDesc.push(conlumn)
						}
						this.loadWords();
					},
					//创建时间排序 end
				//单词管理 end
				
				//词库管理 start
					//注册时间排序 start
					cikuTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.cikuCondition.orderbyAsc.length; i++) {
								if(this.cikuCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.cikuCondition.orderbyDesc.length; i++) {
								if(this.cikuCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.cikuCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.cikuCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.cikuCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.cikuCondition.orderbyDesc.push(conlumn)
						}
						this.loadCikus();
					},
					//注册时间排序 end
				//词库管理 end
				
				//用户管理 start
					//年龄排序 start
					userAgeSort(obj){
						let conlumn ="age";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.userCondition.orderbyAsc.length; i++) {
								if(this.userCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.userCondition.orderbyDesc.length; i++) {
								if(this.userCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.userCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.userCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.userCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.userCondition.orderbyDesc.push(conlumn)
						}
						this.loadUsers();
					},
					//年龄排序 end
					//注册时间排序 start
					userTimeSort(obj){
						let conlumn ="createtime";
						if(obj.order==null){
							//取消排序
							let flag1= false;
							let id1=0;
							let flag2= false;
							let id2=0;
							for (let i = 0; i < this.userCondition.orderbyAsc.length; i++) {
								if(this.userCondition.orderbyAsc[i]==conlumn){
									flag1=true;
									id1=i;
								}
							}
							for (let i = 0; i < this.userCondition.orderbyDesc.length; i++) {
								if(this.userCondition.orderbyDesc[i]==conlumn){
									flag2=true;
									id2=i;
								}
							}
							if(flag1){
								this.userCondition.orderbyAsc.splice(id1,1);
							}
							if(flag2){
								this.userCondition.orderbyDesc.splice(id1,1);
							}
						}
						else if(obj.order.indexOf("asc")>-1){
							// 升序/
							this.userCondition.orderbyAsc.push(conlumn);
							
						}else{
							//降序
							this.userCondition.orderbyDesc.push(conlumn)
						}
						this.loadUsers();
					},
					//注册时间排序 end
				
				//用户管理 end
			//表格table 排序 end		
			
			//表格table 条件筛选 start
				//学习圈管理 start
					//标题筛选 start
					studyCircleTitleFilter(e){
						if(e.filter==""){
							this.studyCircleCondition.titleSearch=null;
						}else{
							this.studyCircleCondition.titleSearch=e.filter;
						}
						this.loadStudyCircles();
					},
					//标题筛选 end
					//标签筛选 start
					studyCircleTagsFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.studyCircleCondition.tagsSelect=null;
							this.loadStudyCircles();
						}else{
							// 筛选/
							this.studyCircleCondition.tagsSelect=[];
							 e.filter.forEach(item=>{
								  this.studyCircleCondition.tagsSelect.push(item);
							 })
							this.loadStudyCircles().then(()=>{
								 this.studycircles.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.tags.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.studyCircleTagsFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//标签筛选 end
					//用户筛选 start
					studyCircleUserFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.studyCircleCondition.userSelect=null;
							this.loadStudyCircles();
						}else{
							// 筛选/
							this.studyCircleCondition.userSelect=[];
							 e.filter.forEach(item=>{
								  this.studyCircleCondition.userSelect.push(item);
							 })
							this.loadStudyCircles().then(()=>{
								 this.studycircles.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.studyCircleUserFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//用户筛选 end
				//学习圈管理 end
				//订单管理 start
					//订单号筛选 start
					orderOrdersFilter(e){
						if(e.filter==""){
							this.orderCondition.ordersSearch=null;
						}else{
							this.orderCondition.ordersSearch=e.filter;
						}
						this.loadOrders();
					},
					//订单号筛选 end
					//商品筛选 start
					orderPrductFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.orderCondition.productSelect=null;
							this.loadOrders();
						}else{
							// 筛选/
							this.orderCondition.productSelect=[];
							 e.filter.forEach(item=>{
								  this.orderCondition.productSelect.push(item);
							 })
							this.loadOrders().then(()=>{
								 this.orders.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.orderProductFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//商品筛选 end
					//用户筛选 start
					orderUserFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.orderCondition.userSelect=null;
							this.loadOrders();
						}else{
							// 筛选/
							this.orderCondition.userSelect=[];
							 e.filter.forEach(item=>{
								  this.orderCondition.userSelect.push(item);
							 })
							this.loadOrders().then(()=>{
								 this.orders.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.orderProductFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//用户筛选 end
				//订单管理 end
				//商品管理 start
					//商品名称筛选 start
					productTitleFilter(e){
						if(e.filter==""){
							this.productCondition.titleSearch=null;
						}else{
							this.productCondition.titleSearch=e.filter;
						}
						this.loadProducts();
					},
					//商品名称筛选 end
					//商家筛选筛选 start
					productMerchantFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.productCondition.merchantSelect=null;
							this.loadProducts();
						}else{
							// 筛选/
							this.productCondition.merchantSelect=[];
							 e.filter.forEach(item=>{
								  this.productCondition.merchantSelect.push(item);
							 })
							this.loadProducts().then(()=>{
								 this.products.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.productMerchantFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//商家筛选 end
				//商品管理 end
				//积分管理 start
					//订单号筛选 start
					zbOrderZbcodeFilter(e){
						if(e.filter==""){
							this.zbOrderCondition.zbcodeSearch=null;
						}else{
							this.zbOrderCondition.zbcodeSearch=e.filter;
						}
						this.loadZbOrders();
					},
					//订单号筛选 end
					//支付宝流水号筛选 start
					zbOrderZborderIdFilter(e){
						if(e.filter==""){
							this.zbOrderCondition.zborderidSearch=null;
						}else{
							this.zbOrderCondition.zborderidSearch=e.filter;
						}
						this.loadZbOrders();
					},
					//支付宝流水号筛选 end
					//zborder status筛选 start
					zbOrderStatusFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.zbOrderCondition.statusSelect=null;
							this.loadZbOrders();
						}else{
							// 筛选/
							this.zbOrderCondition.statusSelect=[];
							 e.filter.forEach(item=>{
								  this.zbOrderCondition.statusSelect.push(item);
							 })
							this.loadZbOrders().then(()=>{
								 this.zborders.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.zbOrderStatusFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//zborder status筛选 end
					//zborder user筛选 start
					zbOrderUserFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.zbOrderCondition.userSelect=null;
							this.loadZbOrders();
						}else{
							// 筛选/
							this.zbOrderCondition.userSelect=[];
							 e.filter.forEach(item=>{
								  this.zbOrderCondition.userSelect.push(item);
							 })
							this.loadZbOrders().then(()=>{
								 this.zborders.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.zbOrderUserFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//zborder user筛选 end
					//score user筛选 start
					scoreUserFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.scoreCondition.userSelect=null;
							this.loadScores();
						}else{
							// 筛选/
							this.scoreCondition.userSelect=[];
							 e.filter.forEach(item=>{
								  this.scoreCondition.userSelect.push(item);
							 })
							this.loadScores().then(()=>{
								 this.scores.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.scoreUserFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//score user筛选 end
					//dsc筛选 start
					scoreSourceDscFilter(e){
						if(e.filter==""){
							this.scoreSourceCondition.dscSearch=null;
						}else{
							this.scoreSourceCondition.dscSearch=e.filter;
						}
						this.loadScoreSources();
					},
					//dsc筛选 end
					//type筛选 start
					scoreSourceTypeFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.scoreSourceCondition.typeSelect=null;
							this.loadScoreSources();
						}else{
							// 筛选/
							this.scoreSourceCondition.typeSelect=[];
							 e.filter.forEach(item=>{
								  this.scoreSourceCondition.typeSelect.push(item);
							 })
							this.loadScoreSources().then(()=>{
								 this.scoreSources.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.type) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.scoreSourceTypeFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//type筛选 end
				//积分管理 end
				//题库管理 start
					//题目筛选 start
					testTitleFilter(e){
						if(e.filter==""){
							this.testCondition.titleSearch=null;
						}else{
							this.testCondition.titleSearch=e.filter;
						}
						this.loadTests();
					},
					//题目筛选 end
					//选项A筛选 start
					testOptaFilter(e){
						if(e.filter==""){
							this.testCondition.optaSearch=null;
						}else{
							this.testCondition.optaSearch=e.filter;
						}
						this.loadTests();
					},
					//选项A筛选 end
					//选项B筛选 start
					testOptbFilter(e){
						if(e.filter==""){
							this.testCondition.optbSearch=null;
						}else{
							this.testCondition.optbSearch=e.filter;
						}
						this.loadTests();
					},
					//选项B筛选 end
					//选项C筛选 start
					testOptcFilter(e){
						if(e.filter==""){
							this.testCondition.optcSearch=null;
						}else{
							this.testCondition.optcSearch=e.filter;
						}
						this.loadTests();
					},
					//选项C筛选 end
					//选项D筛选 start
					testOptdFilter(e){
						if(e.filter==""){
							this.testCondition.optdSearch=null;
						}else{
							this.testCondition.optdSearch=e.filter;
						}
						this.loadTests();
					},
					//选项D筛选 end
					//解析筛选 start
					testAnalyFilter(e){
						if(e.filter==""){
							this.testCondition.analySearch=null;
						}else{
							this.testCondition.analySearch=e.filter;
						}
						this.loadTests();
					},
					//解析筛选 end
					//答案筛选 start
					testAnswerFilter(e){
						if(e.filter.length==0){
							// 取消筛选/
							this.testCondition.answerSelect=null;
							this.loadTests();
						}else{
							// 筛选/
							this.testCondition.answerSelect=[];
							 e.filter.forEach(item=>{
								  this.testCondition.answerSelect.push(item);
							 })
							this.loadTests().then(()=>{
								 this.tests.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.answer) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.testAnswerFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//答案筛选 end
				//题库管理 end
				//阅读管理 start
					//文章筛选 start
					readEssayFilter(e){
						if(e.filter==""){
							this.readCondition.essaySearch=null;
						}else{
							this.readCondition.essaySearch=e.filter;
						}
						this.loadReads();
					},
					//文章筛选 end
					//标题筛选 start
					readNameFilter(e){
						if(e.filter==""){
							this.readCondition.nameSearch=null;
						}else{
							this.readCondition.nameSearch=e.filter;
						}
						this.loadReads();
					},
					//标题筛选 end
					//作者筛选 start
					readAuthorFilter(e){
						if(e.filter==""){
							this.readCondition.authorSearch=null;
						}else{
							this.readCondition.authorSearch=e.filter;
						}
						this.loadReads();
					},
					//作者筛选 end
					//简介筛选 start
					readBriefFilter(e){
						if(e.filter==""){
							this.readCondition.briefSearch=null;
						}else{
							this.readCondition.briefSearch=e.filter;
						}
						this.loadReads();
					},
					//简介筛选 end
				//阅读管理 end
				
				//单词管理 start
					//单词筛选 start
					wordWordFilter(e){
						if(e.filter==""){
							this.wordCondition.wordSearch=null;
						}else{
							this.wordCondition.wordSearch=e.filter;
						}
						this.loadWords();
					},
					//单词筛选 end
					//译文筛选 start
					wordTransFilter(e){
						if(e.filter==""){
							this.wordCondition.transSearch=null;
						}else{
							this.wordCondition.transSearch=e.filter;
						}
						this.loadWords();
					},
					//译文筛选 end
				//单词管理 end
				
				//词库管理 start
					//名称筛选 start
					cikuDscFilter(e){
						if(e.filter==""){
							this.cikuCondition.dscSearch=null;
						}else{
							this.cikuCondition.dscSearch=e.filter;
						}
						this.loadCikus();
					},
					//名称筛选 end
					//简写筛选 start
					cikuDscabbFilter(e){
						if(e.filter==""){
							this.cikuCondition.dscabbSearch=null;
						}else{
							this.cikuCondition.dscabbSearch=e.filter;
						}
						this.loadCikus();
					},
					//简写筛选 end
					//创建者筛选 start
					cikuUserFilter(e){
						
						if(e.filter.length==0){
							// 取消筛选/
							this.cikuCondition.userSelect=null;
							this.loadCikus();
						}else{
							// 筛选/
							this.cikuCondition.userSelect=[];
							 e.filter.forEach(item=>{
								  this.cikuCondition.userSelect.push(item);
							 })
							this.loadCikus().then(()=>{
								 this.cikus.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.user.id) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.cikuUserFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//创建者筛选 end
				//词库管理 end
				//用户管理 start
					//性别筛选 start
					userSexFilter(e){
						
						if(e.filter.length==0){
							// 取消筛选/
							this.userCondition.sexSelect=null;
							this.loadUsers();
						}else{
							// 筛选/
							 this.userCondition.sexSelect=[];
							 e.filter.forEach(item=>{
								  this.userCondition.sexSelect.push(item);
							 })
							this.loadUsers().then(()=>{
								 this.users.forEach(item=>{
									let flag =false;
									let id = 0;
									for (let i = 0; i < e.filter.length; i++) {
										if(e.filter[i]==item.sex) {
											id=e.filter[i];
											flag=true;
											break;
										}
									}
									if(flag){
										this.userSexFilterData.forEach(item=>{
											if(item.value==id){
												item.checked=true
											}
										})
									}
									
								});
							});
						
						}
					},
					//性别筛选 end
					//生日筛选 start
					userBirthDayFilter(e){
						if(e.filter.length==0){
							this.userCondition.birthdayDate=null;
						}else{
							this.userCondition.birthdayDate=[];
							this.userCondition.birthdayDate.push(new Date(e.filter[0]).toLocaleDateString())
							this.userCondition.birthdayDate.push(new Date(e.filter[1]).toLocaleDateString())
						}
						this.loadUsers();
						
					},
					//生日筛选 end
					//用户名筛选 start
					userUsernameFilter(e){
						if(e.filter==""){
							this.userCondition.usernameSearch=null;
						}else{
							this.userCondition.usernameSearch=e.filter;
						}
						this.loadUsers();
					},
					//用户名筛选 end
					//密码筛选 start
					userPasswordFilter(e){
						if(e.filter==""){
							this.userCondition.passwordSearch=null;
						}else{
							this.userCondition.passwordSearch=e.filter;
						}
						this.loadUsers();
					},
					//密码筛选 end
					//昵称筛选 start
					userNameFilter(e){
						if(e.filter==""){
							this.userCondition.nameSearch=null;
						}else{
							this.userCondition.nameSearch=e.filter;
						}
						this.loadUsers();
					},
					//昵称筛选 end
					//电话筛选 start
					userPhoneFilter(e){
						if(e.filter==""){
							this.userCondition.phoneSearch=null;
						}else{
							this.userCondition.phoneSearch=e.filter;
						}
						this.loadUsers();
					},
					//电话筛选 end
					//邮箱筛选 start
					userEmailFilter(e){
						if(e.filter==""){
							this.userCondition.emailSearch=null;
						}else{
							this.userCondition.emailSearch=e.filter;
						}
						this.loadUsers();
					},
					//邮箱筛选 end
				//用户管理 end
			//表格table 条件筛选 end
			
			//按钮 start
				//学习圈管理 start
					//删除多个学习圈 start
					deleteStudyCircleBatch(){
						if(this.chooseStudyCirclesIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseStudyCirclesIndex.forEach(item=>{
							ids.push(this.studycircles[item].studyCircle.id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些学习圈批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminStudyCircle/deleteStudyCircleBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let studycircles=[];
											
											for (let i = 0; i < this.studycircles.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseStudyCirclesIndex.length; j++) {
													if(i==this.chooseStudyCirclesIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													studycircles.push(this.studycircles[i])
												}
											
											}
											this.studycircles = JSON.parse(JSON.stringify(studycircles));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个学习圈 end
					//删除单个学习圈 start
					deleteStudyCircleSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该学习圈[${this.studycircles[index].studyCircle.title.slice(0,10)}.....]删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminStudyCircle/deleteStudyCircleSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											
											id:this.studycircles[index].studyCircle.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.studycircles.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个学习圈 end
				//学习圈管理 end
				//订单管理 start
					//取消弹出订单类型框 start
					cancelOrderstatusPop(){
						this.orderstatusEntity=JSON.parse(JSON.stringify(this.orderstatusEntityBackUp));
						 this.$refs.orderstatuspopup.close();
					},
					//取消弹出订单类型框 end
					//删除订单类型 start
					deleteOrderStatus(){
						uni.showModal({
							title:"删除订单类型",
							content:`您确定要删除[${this.orderModuleMenuTexts[this.orderModuleMenu].dsc}]订单类型吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminOrder/deleteOrderStatus",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.orderModuleMenuTexts[this.orderModuleMenu].id
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.orderModuleMenuTexts.splice(this.orderModuleMenu,1);
												this.orderModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除订单类型 end
					//添加订单类型 start
					insertOrderStatus(){
						this.$refs['orderstatusEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminOrder/insertOrderStatus",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.orderstatusEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.orderModuleMenuTexts.push(data.data);
											this.cancelOrderstatusPop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加订单类型 end
					//修改订单类型 start
					updateOrderStatus(){
						uni.request({
							url:this.globalData.url+"adminOrder/updateOrderStatus",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.orderstatusEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.orderModuleMenuTexts[this.orderModuleMenu]=this.orderstatusEntity;
									this.cancelOrderstatusPop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改订单类型 end
					//添加订单类型弹窗 start
					insertOrderStatusPop(){
						this.orderstatusModal="insert";
						 this.$refs.orderstatuspopup.open();
					},
					//添加订单类型弹窗 end
					//修改订单类型名称弹窗 start
					updateOrderStatusPop(){
						this.orderstatusModal="update";
						this.orderstatusEntity=this.orderModuleMenuTexts[this.orderModuleMenu];
						this.$refs.orderstatuspopup.open();
					},
					//修改订单类型名称弹窗 end
					//收货多个订单 start
					receiveOrderBatch(){
						if(this.chooseOrdersIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseOrdersIndex.forEach(item=>{
							ids.push(this.orders[item].order.id)
						})
						uni.showModal({
							title:"收货订单",
							content:`您确定要将这些订单批量收货吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminOrder/recieveOrderBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"收货成功"
											})
											 let orders=[];
											
											for (let i = 0; i < this.orders.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseOrdersIndex.length; j++) {
													if(i==this.chooseOrdersIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													orders.push(this.orders[i])
												}
											
											}
											this.orders = JSON.parse(JSON.stringify(orders));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"收货失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//收货多个订单 end
					//收货单个订单 start
					receiveOrderSingle(index){
						uni.showModal({
							title:"收货订单",
							content:`您确定要将该订单[${this.orders[index].order.orders.length<10?this.orders[index].order.orders:this.orders[index].order.orders.slice(0,10)+'.....'}]收货吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminOrder/recieveOrderSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{											
											id:this.orders[index].order.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"收货成功"
											})
											this.orders.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"收货失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//收货单个订单 end
					//发货多个订单 start
					sendOutOrderBatch(){
						if(this.chooseOrdersIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseOrdersIndex.forEach(item=>{
							ids.push(this.orders[item].order.id)
						})
						uni.showModal({
							title:"发货订单,您确定要将这些订单批量发货吗？",
							placeholderText:"请输入物流单号,用逗号分隔",
							editable:true,
							success: (res) => {
								if(res.confirm){
									if(res.content.trim()==''){
										uni.showToast({
											title:"物流单号不能为空！"
										});
										return
									}
									uni.request({
										url:this.globalData.url+"adminOrder/sendOutOrderBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											ids:ids,
											trackids:res.content.trim()
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"发货成功"
											})
											 let orders=[];
											
											for (let i = 0; i < this.orders.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseOrdersIndex.length; j++) {
													if(i==this.chooseOrdersIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													orders.push(this.orders[i])
												}
											
											}
											this.orders = JSON.parse(JSON.stringify(orders));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"发货失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//发货多个订单 end
					//发货单个订单 start
					sendOutOrderSingle(index){
						uni.showModal({
							title:`发货订单，您确定要将该订单[${this.orders[index].order.orders.length<10?this.orders[index].order.orders:this.orders[index].order.orders.slice(0,10)+'.....'}]发货吗？`,
							placeholderText:"请输入物流单号",
							editable:true,
							success: (res) => {
								if(res.confirm){
									if(res.content.trim()==''){
										uni.showToast({
											title:"物流单号不能为空！"
										});
										return
									}
									uni.request({
										url:this.globalData.url+"adminOrder/sendOutOrderSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{											
											id:this.orders[index].order.id,
											trackid:res.content.trim()
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"发货成功"
											})
											this.orders.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"发货失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//发货单个订单 end
					//取消多个订单 start
					cancelOrderBatch(){
						if(this.chooseOrdersIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseOrdersIndex.forEach(item=>{
							ids.push(this.orders[item].order.id)
						})
						uni.showModal({
							title:"取消订单",
							content:`您确定要将这些订单批量取消吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminOrder/cancelOrderBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"取消成功"
											})
											 let orders=[];
											
											for (let i = 0; i < this.orders.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseOrdersIndex.length; j++) {
													if(i==this.chooseOrdersIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													orders.push(this.orders[i])
												}
											
											}
											this.orders = JSON.parse(JSON.stringify(orders));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"取消失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//取消多个订单 end
					//取消单个订单 start
					cancelOrderSingle(index){
						uni.showModal({
							title:"取消订单",
							content:`您确定要将该订单[${this.orders[index].order.orders.length<10?this.orders[index].order.orders:this.orders[index].order.orders.slice(0,10)+'.....'}]取消吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminOrder/cancelOrderSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{											
											id:this.orders[index].order.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"取消成功"
											})
											this.orders.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"取消失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//取消单个订单 end
				//订单管理 end
				//商品管理 start
					//取消弹出商品二级类别框 start
					cancelPtypeSubPop(){
						this.ptypeSubEntity=JSON.parse(JSON.stringify(this.ptypeSubEntityBackUp));
						 this.$refs.ptypesubpopup.close();
					},
					//取消弹出商品二级类别框 end
					//删除商品二级类别 start
					deletePTypeSub(){
						uni.showModal({
							title:"删除商品二级类别",
							content:`您确定要删除[${this.productSubModuleMenuTexts[this.productSubModuleMenu].dsc}]商品二级类别吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminProduct/deletePTypeSub",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.productSubModuleMenuTexts[this.productSubModuleMenu].id,
											pType:this.productModuleMenuTexts[this.productModuleMenu].id,
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.productSubModuleMenuTexts.splice(this.productSubModuleMenu,1);
												this.productSubModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除商品二级类别 end
					//添加商品二级类别 start
					insertPTypeSub(){
						this.$refs['ptypeSubEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminProduct/insertPTypeSub",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.ptypeSubEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.productSubModuleMenuTexts.push(data.data);
											this.cancelPtypeSubPop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加商品二级类别 end
					//修改商品二级类别 start
					updatePTypeSub(){
						uni.request({
							url:this.globalData.url+"adminProduct/updatePTypeSub",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.ptypeSubEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.productSubModuleMenuTexts[this.productSubModuleMenu]=this.ptypeSubEntity;
									this.cancelPtypeSubPop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改商品二级类别 end
					//添加商品二级类别弹窗 start
					insertPTypeSubPop(){
						this.ptypeSubModal="insert";
						this.ptypeSubEntity.ptypeid=this.productModuleMenuTexts[this.productModuleMenu].id;
						 this.$refs.ptypesubpopup.open();
					},
					//添加商品二级类别弹窗 end
					//修改商品二级类别弹窗 start
					updatePTypeSubPop(){
						this.ptypeSubModal="update";
						this.ptypeSubEntity=this.productSubModuleMenuTexts[this.productSubModuleMenu];
						this.$refs.ptypesubpopup.open();
					},
					//修改商品二级类别弹窗 end
					//取消弹出商品一级类别框 start
					cancelPtypePop(){
						this.ptypeEntity=JSON.parse(JSON.stringify(this.ptypeEntityBackUp));
						 this.$refs.ptypepopup.close();
					},
					//取消弹出商品一级类别框 end
					//删除商品一级类别 start
					deletePType(){
						uni.showModal({
							title:"删除商品一级类别",
							content:`您确定要删除[${this.productModuleMenuTexts[this.productModuleMenu].dsc}]商品一级类别吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminProduct/deletePType",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.productModuleMenuTexts[this.productModuleMenu].id
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.productModuleMenuTexts.splice(this.productModuleMenu,1);
												this.productModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除商品一级类别 end
					//添加商品一级类别 start
					insertPType(){
						this.$refs['ptypeEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminProduct/insertPType",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.ptypeEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.productModuleMenuTexts.push(data.data);
											this.cancelPtypePop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加商品一级类别 end
					//修改商品一级类别 start
					updatePType(){
						uni.request({
							url:this.globalData.url+"adminProduct/updatePType",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.ptypeEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.productModuleMenuTexts[this.productModuleMenu]=this.ptypeEntity;
									this.cancelPtypePop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改商品一级类别 end
					//添加商品一级类别弹窗 start
					insertPTypePop(){
						this.ptypeModal="insert";
						 this.$refs.ptypepopup.open();
					},
					//添加商品一级类别弹窗 end
					//修改商品一级类别弹窗 start
					updatePTypePop(){
						this.ptypeModal="update";
						this.ptypeEntity=this.productModuleMenuTexts[this.productModuleMenu];
						this.$refs.ptypepopup.open();
					},
					//修改商品一级类别弹窗 end
					//删除多个商品 start
					deleteProductBatch(){
						if(this.chooseProductsIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseProductsIndex.forEach(item=>{
							ids.push(this.products[item].product.id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些商品批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminProduct/deleteProductBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											pType:this.productModuleMenuTexts[this.productModuleMenu].id,
											pTypeSub:this.productSubModuleMenuTexts[this.productSubModuleMenu].id,
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let products=[];
											
											for (let i = 0; i < this.products.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseProductsIndex.length; j++) {
													if(i==this.chooseProductsIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													products.push(this.products[i])
												}
											
											}
											this.products = JSON.parse(JSON.stringify(products));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个商品 end
					//删除单个商品 start
					deleteProductSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该商品[${this.products[index].product.title.slice(0,10)}.....]删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminProduct/deleteProductSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											pType:this.productModuleMenuTexts[this.productModuleMenu].id,
											pTypeSub:this.productSubModuleMenuTexts[this.productSubModuleMenu].id,
											id:this.products[index].product.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.products.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个商品 end
				//商品管理 end
				
				//积分管理 start
					//取消弹出充值框 start
					cancelZbOrderPop(){
						this.zbOrderEntity = JSON.parse(JSON.stringify(this.zbOrderEntityBackUp));
						 this.$refs.zborderpopup.close();
					},
					//取消弹出充值框 end
					//修改充值 start
					updateZbOrder(){
						this.$refs['zbOrderEntity']
							.validate()
							.then(res => {
								if(this.zbOrderEntity.zborder.money==null||this.zbOrderEntity.zborder.money==0){
									uni.showToast({
										title:"积分错误"
									});
									return;
								}
								uni.showModal({
									title:"注意",
									content:`确定要为用户${this.zbOrderEntity.user.username}充值${this.zbOrderEntity.zborder.money*10}积分，需要${this.zbOrderEntity.zborder.money}元`,
									success: (res) => {
										if(res.confirm){
												 let src=`${this.globalData.url}pay/alipay?uid=${this.zbOrderEntity.user.id}&money=${this.zbOrderEntity.zborder.money}&dsc=${this.zbOrderEntity.zborder.money*10}`;
												uni.navigateTo({
													url:"/pages/pay/pay?src="+encodeURIComponent(src)
												});
										}
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//修改充值 end	
					//修改充值弹窗 start
					updateZbOrderPop(index){		
						this.zbOrderEntity.user=this.zborders[index].user;
						
						this.$refs.zborderpopup.open();
					},
					//修改充值弹窗 end
					//取消弹出积分框 start
					cancelScorePop(){				
						 this.$refs.scorepopup.close();
					},
					//取消弹出积分框 end
					//修改积分 start
					updateScore(){
						uni.request({
							url:this.globalData.url+"adminScore/updateScore",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.scoreEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									
									this.cancelScorePop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改积分 end	
					//修改积分弹窗 start
					updateScorePop(index){		
						this.scoreEntity=this.scores[index].score;
						this.$refs.scorepopup.open();
					},
					//修改积分弹窗 end
				//积分管理 end
				//题库管理 start
					//取消弹出题库框 start
					cancelTesttypePop(){
						this.testtypeEntity=JSON.parse(JSON.stringify(this.testtypeEntityBackUp));
						 this.$refs.testtypepopup.close();
					},
					//取消弹出题库框 end
					//删除题库 start
					deleteTestType(){
						uni.showModal({
							title:"删除题库",
							content:`您确定要删除[${this.testModuleMenuTexts[this.testModuleMenu].dsc}]题库吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminTest/deleteTestType",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.testModuleMenuTexts[this.testModuleMenu].id
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.testModuleMenuTexts.splice(this.testModuleMenu,1);
												this.testModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除题库 end
					//添加题库 start
					insertTestType(){
						this.$refs['testtypeEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminTest/insertTestType",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.testtypeEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.testModuleMenuTexts.push(data.data);
											this.cancelTesttypePop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加题库 end
					//修改题库 start
					updateTestType(){
						uni.request({
							url:this.globalData.url+"adminTest/updateTestType",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.testtypeEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.testModuleMenuTexts[this.testModuleMenu]=this.testtypeEntity;
									this.cancelTesttypePop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改题库 end
					//添加题库弹窗 start
					insertTestTypePop(){
						this.testtypeModal="insert";
						 this.$refs.testtypepopup.open();
					},
					//添加题库弹窗 end
					//修改题库名称弹窗 start
					updateTestTypePop(){
						this.testtypeModal="update";
						this.testtypeEntity=this.testModuleMenuTexts[this.testModuleMenu];
						this.$refs.testtypepopup.open();
					},
					//修改题库名称弹窗 end
					//删除多个题目 start
					deleteTestBatch(){
						if(this.chooseTestsIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseTestsIndex.forEach(item=>{
							ids.push(this.tests[item].id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些题目批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminTest/deleteTestBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											testType:this.testModuleMenuTexts[this.testModuleMenu].id,
											
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let tests=[];
											
											for (let i = 0; i < this.tests.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseTestsIndex.length; j++) {
													if(i==this.chooseTestsIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													tests.push(this.tests[i])
												}
											
											}
											this.tests = JSON.parse(JSON.stringify(tests));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个题目 end
					//删除单个题目 start
					deleteTestSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该题目[${this.tests[index].title.length<10?this.tests[index].title:this.tests[index].title.slice(0,10)+'.....'}]删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminTest/deleteTestSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											testType:this.testModuleMenuTexts[this.testModuleMenu].id,
											
											id:this.tests[index].id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.tests.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个题目 end
				//题库管理 end
				
				
				//阅读管理 start
					//取消弹出阅读二级类别框 start
					cancelReadtypeSubPop(){
						this.readtypeSubEntity=JSON.parse(JSON.stringify(this.readtypeSubEntityBackUp));
						 this.$refs.readtypesubpopup.close();
					},
					//取消弹出阅读二级类别框 end
					//删除阅读二级类别 start
					deleteReadTypeSub(){
						uni.showModal({
							title:"删除阅读二级类别",
							content:`您确定要删除[${this.readSubModuleMenuTexts[this.readSubModuleMenu].dsc}]阅读二级类别吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminRead/deleteReadTypeSub",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											readType:this.readModuleMenuTexts[this.readModuleMenu].id,
											id:this.readSubModuleMenuTexts[this.readSubModuleMenu].id,
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.readSubModuleMenuTexts.splice(this.readSubModuleMenu,1);
												this.readSubModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除阅读二级类别 end
					//添加阅读二级类别 start
					insertReadTypeSub(){
						this.$refs['readtypeSubEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminRead/insertReadTypeSub",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.normalData,
										'token':uni.getStorageSync('token')
									},
									data:{
										readType:this.readModuleMenuTexts[this.readModuleMenu].id,
										readTypeSub:JSON.stringify(this.readtypeSubEntity)
									},
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.readSubModuleMenuTexts.push(data.data);
											this.cancelReadtypeSubPop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加阅读二级类别 end
					//修改阅读二级类别 start
					updateReadTypeSub(){
						uni.request({
							url:this.globalData.url+"adminRead/updateReadTypeSub",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.normalData,
								'token':uni.getStorageSync('token')
							},
							data:{
								readType:this.readModuleMenuTexts[this.readModuleMenu].id,
								readTypeSub:JSON.stringify(this.readtypeSubEntity)
							},
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.readSubModuleMenuTexts[this.readSubModuleMenu]=this.readtypeSubEntity;
									this.cancelReadtypeSubPop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改阅读二级类别 end
					//添加阅读二级类别弹窗 start
					insertReadTypeSubPop(){
						this.readtypeSubModal="insert";
						 this.$refs.readtypesubpopup.open();
					},
					//添加阅读二级类别弹窗 end
					//修改阅读二级类别弹窗 start
					updateReadTypeSubPop(){
						this.readtypeSubModal="update";
						this.readtypeSubEntity=this.readSubModuleMenuTexts[this.readSubModuleMenu];
						this.$refs.readtypesubpopup.open();
					},
					//修改阅读二级类别弹窗 end
					//取消弹出阅读一级类别框 start
					cancelReadtypePop(){
						this.readtypeEntity=JSON.parse(JSON.stringify(this.readtypeEntityBackUp));
						 this.$refs.readtypepopup.close();
					},
					//取消弹出阅读一级类别框 end
					//删除阅读一级类别 start
					deleteReadType(){
						uni.showModal({
							title:"删除阅读一级类别",
							content:`您确定要删除[${this.readModuleMenuTexts[this.readModuleMenu].dsc}]阅读一级类别吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminRead/deleteReadType",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.readModuleMenuTexts[this.readModuleMenu].id
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.readModuleMenuTexts.splice(this.readModuleMenu,1);
												this.readModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除阅读一级类别 end
					//添加阅读一级类别 start
					insertReadType(){
						this.$refs['readtypeEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminRead/insertReadType",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.readtypeEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.readModuleMenuTexts.push(data.data);
											this.cancelReadtypePop();
					
										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加阅读一级类别 end
					//修改阅读一级类别 start
					updateReadType(){
						uni.request({
							url:this.globalData.url+"adminRead/updateReadType",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.readtypeEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.readModuleMenuTexts[this.readModuleMenu]=this.readtypeEntity;
									this.cancelReadtypePop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改阅读一级类别 end
					//添加阅读一级类别弹窗 start
					insertReadTypePop(){
						this.readtypeModal="insert";
						 this.$refs.readtypepopup.open();
					},
					//添加阅读一级类别弹窗 end
					//修改阅读一级类别弹窗 start
					updateReadTypePop(){
						this.readtypeModal="update";
						this.readtypeEntity=this.readModuleMenuTexts[this.readModuleMenu];
						this.$refs.readtypepopup.open();
					},
					//修改阅读一级类别弹窗 end
					//删除多个文章 start
					deleteReadBatch(){
						if(this.chooseReadsIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseReadsIndex.forEach(item=>{
							ids.push(this.reads[item].id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些文章批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminRead/deleteReadBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											readType:this.readModuleMenuTexts[this.readModuleMenu].id,
											readTypeSub:this.readSubModuleMenuTexts[this.readSubModuleMenu].id,
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let reads=[];
											
											for (let i = 0; i < this.reads.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseReadsIndex.length; j++) {
													if(i==this.chooseReadsIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													reads.push(this.reads[i])
												}
											
											}
											this.reads = JSON.parse(JSON.stringify(reads));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个文章 end
					//删除单个文章 start
					deleteReadSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该文章[${this.reads[index].name}]删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminRead/deleteReadSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											readType:this.readModuleMenuTexts[this.readModuleMenu].id,
											readTypeSub:this.readSubModuleMenuTexts[this.readSubModuleMenu].id,
											id:this.reads[index].id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.reads.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个文章 end
				//阅读管理 end
				
				
				//单词管理 start
					//删除多个单词 start
					deleteWordBatch(){
						if(this.chooseWordsIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseWordsIndex.forEach(item=>{
							ids.push(this.words[item].word.id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些单词批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminWord/deleteWordBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											initial:this.wordModuleMenuTexts[this.wordModuleMenu].text.toLowerCase(),
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let words=[];
											
											for (let i = 0; i < this.words.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseWordsIndex.length; j++) {
													if(i==this.chooseWordsIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													words.push(this.words[i])
												}
											
											}
											this.words = JSON.parse(JSON.stringify(words));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个单词 end
					//删除单个单词 start
					deleteWordSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该单词[${this.words[index].word.word}]删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminWord/deleteWordSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											initial:this.wordModuleMenuTexts[this.wordModuleMenu].text.toLowerCase(),
											id:this.words[index].word.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.words.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个单词 end
				//单词管理 end
				
			
				//词库管理 start
					//删除多个词库 start
					deleteCikuBatch(){
						if(this.chooseCikusIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseCikusIndex.forEach(item=>{
							ids.push(this.cikus[item].ciku.id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些词库批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminCiku/deleteCikuBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											cikuTypeId:this.cikuModuleMenuTexts[this.cikuModuleMenu].id,
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let cikus=[];
											
											for (let i = 0; i < this.cikus.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseCikusIndex.length; j++) {
													if(i==this.chooseCikusIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													cikus.push(this.cikus[i])
												}
											
											}
											this.cikus = JSON.parse(JSON.stringify(cikus));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个词库 end
					//删除单个词库 start
					deleteCikuSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该[${this.cikus[index].ciku.dsc}]词库删除吗？`,
							success: (res) => {
								if(res.confirm){
								
									uni.request({
										url:this.globalData.url+"adminCiku/deleteCikuSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											cikuTypeId:this.cikuModuleMenuTexts[this.cikuModuleMenu].id,
											id:this.cikus[index].ciku.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.cikus.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个词库 end
					//取消弹出词库类别框 start
					cancelCikutypePop(){
						this.cikutypeEntity=JSON.parse(JSON.stringify(this.cikutypeEntityBackUp));
						 this.$refs.cikutypepopup.close();
					},
					//取消弹出词库类别框 end
					//删除词库类别 start
					deleteCikuType(){
						uni.showModal({
							title:"删除词库类别",
							content:`您确定要删除[${this.cikuModuleMenuTexts[this.cikuModuleMenu].dsc}]词库类别吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminCiku/deleteCikuType",
										dataType:'json',
										method:'POST',
										header:{
											'content-type':this.globalData.contentType.normalData,
											'token':uni.getStorageSync('token')
										},
										data:{
											id:this.cikuModuleMenuTexts[this.cikuModuleMenu].id
										},
										success: (res) => {
											let data = res.data;
											if(data.code==200){
												
												uni.showToast({
													title:"删除成功"
												})
									
												this.cikuModuleMenuTexts.splice(this.cikuModuleMenu,1);
												this.cikuModuleMenu--;
												
												
											}else{
												uni.showToast({
													title:"删除失败"
												});
												console.log(data.message);
											}
										},
										fail: (e) => {
											console.log(e);
											
										}
									})
								}
							}
						})
					},
					//删除词库类别 end
					//添加词库类别 start
					insertCikuType(){
						this.$refs['cikutypeEntity']
							.validate()
							.then(res => {
								
								uni.request({
									url:this.globalData.url+"adminCiku/insertCikuType",
									dataType:'json',
									method:'POST',
									header:{
										'content-type':this.globalData.contentType.jsonData,
										'token':uni.getStorageSync('token')
									},
									data:this.cikutypeEntity,
									success: (res) => {
										let data = res.data;
										if(data.code==200){			
											uni.showToast({
												title:"添加成功"
											})
											this.cikuModuleMenuTexts.push(data.data);
											this.cancelCikutypePop();

										}else{
											uni.showToast({
												title:"添加失败"
											});
											console.log(data.message);
										}
									},
									fail: (e) => {
										console.log(e);
										
									}
								})
								
							}).catch(err => {
								
								console.log('表单错误信息：', err);
							});
					},
					//添加词库类别 end
					//修改词库类别 start
					updateCikuType(){
						uni.request({
							url:this.globalData.url+"adminCiku/updateCikuType",
							dataType:'json',
							method:'POST',
							header:{
								'content-type':this.globalData.contentType.jsonData,
								'token':uni.getStorageSync('token')
							},
							data:this.cikutypeEntity,
							success: (res) => {
								let data = res.data;
								if(data.code==200){
									
									uni.showToast({
										title:"修改成功"
									})
									this.cikuModuleMenuTexts[this.cikuModuleMenu]=this.cikutypeEntity;
									this.cancelCikutypePop();
									
									
									
								}else{
									uni.showToast({
										title:"修改失败"
									});
									console.log(data.message);
								}
							},
							fail: (e) => {
								console.log(e);
								
							}
						})
					},
					//修改词库类别 end
					//添加词库类别弹窗 start
					insertCikuTypePop(){
						this.cikutypeModal="insert";
						 this.$refs.cikutypepopup.open();
					},
					//添加词库类别弹窗 end
					//修改词库类别弹窗 start
					updateCikuTypePop(){
						this.cikutypeModal="update";
						this.cikutypeEntity=this.cikuModuleMenuTexts[this.cikuModuleMenu];
						this.$refs.cikutypepopup.open();
					},
					//修改词库类别弹窗 end
				//词库管理 end
			
				//用户管理 start
					//删除多个用户 start
					deleteUserBatch(){
						if(this.chooseUsersIndex.length==0){
							uni.showToast({
								title:"未选中行"
							});
							return;
						}
						let ids = [];
						this.chooseUsersIndex.forEach(item=>{
							ids.push(this.users[item].id)
						})
						uni.showModal({
							title:"删除",
							content:`您确定要将这些${this.userModuleMenu==0?'用户':'管理员'}批量删除吗？`,
							success: (res) => {
								if(res.confirm){
									uni.request({
										url:this.globalData.url+"adminUser/deleteBatch",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											ids:ids
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											 let users=[];
											
											for (let i = 0; i < this.users.length; i++) {
												let flag =true;
												for (let j = 0; j < this.chooseUsersIndex.length; j++) {
													if(i==this.chooseUsersIndex[j]){
														flag=false;
														break;
													}
												}
												if(flag){
													users.push(this.users[i])
												}
											
											}
											this.users = JSON.parse(JSON.stringify(users));
											
											
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除多个用户 end
					//删除单个用户 start
					deleteUserSignle(index){
						uni.showModal({
							title:"删除",
							content:`您确定要将该${this.userModuleMenu==0?'用户':'管理员'}删除吗？`,
							success: (res) => {
								if(res.confirm){
									let user =this.users[index];
									console.log(user);
									uni.request({
										url:this.globalData.url+"adminUser/deleteSingle",
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											id:user.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"删除成功"
											})
											this.users.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"删除失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
					},
					//删除单个用户 end
					
					//用户权限 start
					userPermissionChange(index){
						uni.showModal({
							title:"变更权限",
							content:`您确定要将该${this.userModuleMenu==0?'用户':'管理员'}权限变更为${this.userModuleMenu==0?'管理员':'用户'}权限？`,
							success: (res) => {
								if(res.confirm){
									let user =this.users[index];
									console.log(user);
									let url = `${this.userModuleMenu==0?'toAdmin':'toNormalUser'}`;
									uni.request({
										url:this.globalData.url+"adminUser/"+url,
										method:'POST',
										header:this.globalData.header.normal,
										dataType:"json",
										data:{
											id:user.id
										} 
									}).then((res)=>{
										let data =res.data;
										if (data.code == 200) {
											// console.log(this.globalData.Base64.decode( data.data.sentences));
										
											uni.showToast({
												title:"更新权限成功"
											})
											this.users.splice(index,1);
											
											
										} else if (data.code == 520) {
											this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
											
										} else {
											console.log(data.message);
											uni.showToast({
												title:"更新权限失败"
											})
										}
									}).catch((e)=>{
										console.log(e);
										reject(e);
									})
								}
							}
						})
						
					},
					//用户权限 end
				//用户管理 end
			
				//首页设置 start
					//确定修改 start
					confirmUpdateSetting(){
						this.$refs['indexSetting']
							.validate()
							.then(res => {
								console.log(this.indexSetting);
								this.indexSetting.updateAuthor = JSON.parse(uni.getStorageSync('user')).username;
								let wordSetting = this.indexSetting;
								uni.request({
									url:this.globalData.url+"adminFile/updateWordSetting",
									method:'POST',
									header:this.globalData.header.json,
									dataType:"json",
									data:wordSetting,
								}).then((res)=>{
									let data =res.data;
									if (data.code == 200) {
										// console.log(this.globalData.Base64.decode( data.data.sentences));
										uni.setStorageSync("menu",1);
										uni.reLaunch({
											url:"/pages/index/index"
										});
									} else if (data.code == 520) {
										this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
										
									} else {
										console.log(data.message);
										
									}
								}).catch((e)=>{
									console.log(e);
									reject(e);
								})
							}).catch(err => {
								uni.showToast({
									icon: 'error',
									title: '数据格式错误'
								});
								console.log('表单错误信息：', err);
							});
					},
					//确定修改 end
					//恢复默认 start
					cleanDefaultSetting(){
						this.indexSetting=JSON.parse(JSON.stringify(this.indexSettingBackUp));
					
					},
					//恢复默认 end
					//随机每日一词 start
					randomEveryDayWord(){
						uni.request({
								url:this.globalData.url+"word/getRandomOne",
								method:'POST',
								dataType:'json',
								header:this.globalData.header.normal,
								
								success: (res) => {
									let data =res.data;
									if(data.code==200){

										this.setWordToEveryWordSetting(data.data);
									
									}else{
										console.log(data.message);
										
									}
								},
								fail: (error) => {
									console.log(error);
									
								}
						});
					},
					//随机每日一次 end
					//将首页设置恢复原始 start
					toOriginSetting(){
						uni.request({
							url:this.globalData.url+"adminFile/updateWordSettingToOrigin",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								
								uni.setStorageSync("menu",1);
								uni.reLaunch({
									url:"/pages/index/index"
								});
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								
							} else {
								console.log(data.message);
								
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})
					},
					//将首页设置恢复原始 end
				//首页设置 end
			//按钮 end
			
		
			
			
			//加载数据 start
			//加载标签 start
			loadTags(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminStudyCircle/getTags",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.studyCircleTagsFilterData=[];
								data.data.forEach((item)=>{
									this.studyCircleTagsFilterData.push({
										text:item.dsc,
										value:item.id,
									});
								})
								
								console.log(this.studyCircleTagsFilterData);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载标签 end
			//加载学习圈 start
			loadStudyCircles(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminStudyCircle/getStudyCircle",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.studyCircleCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.studycircles =  this.globalData.Base64.decode(data.data.one);					
								this.studyCircleCondition.current =data.data.current;
								this.studyCircleCondition.total=data.data.total;
								this.studyCircleCondition.size=data.data.pageSize;
								this.studycircles.forEach((item,index)=>{
									
									item.user.headimage = this.globalData.url+"static/image/headImage/"+item.user.headimage;
									
								});
								console.log(this.studycircles);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载学习圈 end
			//加载各种数据之和 start
			loadAllDatas(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminData/getAllData",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.allDatas =  data.data
								console.log(this.allDatas);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载各种数据之和 end
			
			//加载各状态下的订单数量 start
			loadOrderNums(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminOrder/getOrderNums",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.ordersNums =  data.data
								console.log(this.ordersNums);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载各状态下的订单数量 end
			//加载订单的商品 start
			loadOrderProducts(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminOrder/getOrderProducts",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								orderstatus:this.orderCondition.orderstatus,
							},
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								data.data.forEach((item)=>{
									this.orderProductFilterData.push({
										text:'商品：'+item.title.slice(0,20)+'......',
										value:item.id,
									});
								})
								console.log(this.orderProductFilterData);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载订单的商品 end
			//加载订单 start
			loadOrders(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminOrder/getOrder",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.orderCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.orders = data.data.data;
															
								this.orderCondition.current =data.data.current;
								this.orderCondition.total=data.data.total;
								this.orderCondition.size=data.data.pageSize;
								this.orders.forEach((item,index)=>{
									item.product.product.imgAddre =item.product.product.imgAddre.indexOf('//')==-1?(this.globalData.url+'static/image/productImage/'+item.product.product.imgAddre):item.product.product.imgAddre
									
									item.product.user.headimage = this.globalData.url+"static/image/headImage/"+item.product.user.headimage;
									item.user.headimage = this.globalData.url+"static/image/headImage/"+item.user.headimage;
									if(item.logistics!=null){
										let datas =[];
										// for (let i = item.logistics.result.list.length-1; i >=0 ; i--) {
										// 	datas.push({title:item.logistics.result.list[i].status,desc:item.logistics.result.list[i].time})	
										// }
										for (let i = 0; i <item.logistics.result.list.length ; i++) {
											datas.push({title:item.logistics.result.list[i].status,desc:item.logistics.result.list[i].time})	
										}
										item.logistics.result.data=datas;
										
									}
								});
								console.log(this.orders);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载订单 end
			//加载订单类型 start
			loadOrderStatus(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminOrder/getOrderStatus",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.orderModuleMenuTexts=data.data;
								console.log(this.orderModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载订单类型 end
			//加载商家 start
			loadAllMerchants(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminUser/getAllMerchants",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.productMerchantFilterData=[];
								data.data.forEach((item)=>{
									this.productMerchantFilterData.push({
										text:'商家：'+item.username,
										value:item.id,
									});
								})
								
								console.log(this.productMerchantFilterData);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载商家 end
			//加载商品一类类别 start
			loadPType(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminProduct/getPType",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.productModuleMenuTexts=data.data;
								console.log(this.productModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载商品一类类别 end
			//加载商品二类类别 start
			loadPTypeSub(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminProduct/getPTypeSub",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								pType:this.productModuleMenuTexts[this.productModuleMenu].id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.productSubModuleMenuTexts=data.data;
								this.productCondition.pTypeSub=this.productSubModuleMenuTexts[this.productSubModuleMenu].id;
								console.log(this.productSubModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载商品二类类别 end
			//加载商品 start
			loadProducts(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminProduct/getProduct",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.productCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.products = data.data.data;
															
								this.productCondition.current =data.data.current;
								this.productCondition.total=data.data.total;
								this.productCondition.size=data.data.pageSize;
								this.products.forEach((item,index)=>{
									item.product.imgAddre =item.product.imgAddre.indexOf('//')==-1?(this.globalData.url+'static/image/productImage/'+item.product.imgAddre):item.product.imgAddre
									
									item.user.headimage = this.globalData.url+"static/image/headImage/"+item.user.headimage;
								});
								console.log(this.products);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载商品 end
			//加载流水 start
			loadZbOrders(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminScore/getZbOrder",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.zbOrderCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.zborders = data.data.data;
								this.zborders.forEach((item)=>{
									item.user.headimage = this.globalData.url+"static/image/headImage/"+item.user.headimage;
								})							
								this.zbOrderCondition.current =data.data.current;
								this.zbOrderCondition.total=data.data.total;
								this.zbOrderCondition.size=data.data.pageSize;
								
								console.log(this.zborders);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载流水 end
			//加载积分 start
			loadScores(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminScore/getScore",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.scoreCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.scores = data.data.data;
								this.scores.forEach((item)=>{
									item.user.headimage = this.globalData.url+"static/image/headImage/"+item.user.headimage;
								})							
								this.scoreCondition.current =data.data.current;
								this.scoreCondition.total=data.data.total;
								this.scoreCondition.size=data.data.pageSize;
								
								console.log(this.scores);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载积分 end
			//加载所有用户管理员 start
			loadAllUsers(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminUser/getAllUserNoPageNoType",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.scoreUserFilterData=[];
								this.zbOrderUserFilterData=[];
								this.orderUserFilterData=[];
								this.studyCircleUserFilterData=[];
								data.data.forEach((item,index)=>{
									this.scoreUserFilterData.push({
										text:'用户：'+item.username,
										value:item.id,
									});
									this.zbOrderUserFilterData.push({
										text:'用户：'+item.username,
										value:item.id,
									});
									this.orderUserFilterData.push({
										text:'用户：'+item.username,
										value:item.id,
									});
									this.studyCircleUserFilterData.push({
										text:'用户：'+item.username,
										value:item.id,
									});
								})
								
								console.log(this.scoreUserFilterData);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载所有用户管理员 end
			//加载积分来源类别 start
			loadScoreSources(){
				if(uni.getStorageSync("scoreSourceCurrent")!=''){
					this.scoreSourceCondition.current=parseInt(uni.getStorageSync("scoreSourceCurrent"));
					uni.removeStorageSync("scoreSourceCurrent");
				}
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminScore/getScoreSourceType",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.scoreSourceCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.scoreSources = data.data.data;
															
								this.scoreSourceCondition.current =data.data.current;
								this.scoreSourceCondition.total=data.data.total;
								this.scoreSourceCondition.size=data.data.pageSize;
								
								console.log(this.scoreSources);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载积分来源类别 end
			//加载题目 start
			loadTests(){
				if(uni.getStorageSync("testType")!=''){
					this.testCondition.testType=parseInt(uni.getStorageSync("testType"));
					this.testModuleMenuTexts.forEach((item,index)=>{
						if(item.id==this.testCondition.testType){
							this.testModuleMenu=index;
						}
					})
					uni.removeStorageSync("testType");
				}
				
				if(uni.getStorageSync("testCurrent")!=''){
					this.testCondition.current=parseInt(uni.getStorageSync("testCurrent"));
					uni.removeStorageSync("testCurrent");
				}
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminTest/getTest",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.testCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.tests = data.data.data;
															
								this.testCondition.current =data.data.current;
								this.testCondition.total=data.data.total;
								this.testCondition.size=data.data.pageSize;
								
								console.log(this.tests);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载题目 end
			//加载题库 start
			loadTestType(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminTest/getTestType",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.testModuleMenuTexts=data.data;
								console.log(this.testModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载题库 end
			//加载文章一类类别 start
			loadReadType(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminRead/getReadType",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.readModuleMenuTexts=data.data;
								console.log(this.readModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载文章一类类别 end
			//加载文章二类类别 start
			loadReadTypeSub(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminRead/getReadTypeSub",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								readType:this.readModuleMenuTexts[this.readModuleMenu].id
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.readSubModuleMenuTexts=data.data;
								console.log(this.readSubModuleMenuTexts);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载文章二类类别 end
			//加载文章 start
			loadReads(){
				if(uni.getStorageSync("readType")!=''){
					this.readCondition.readType=parseInt(uni.getStorageSync("readType"));
					this.readModuleMenuTexts.forEach((item,index)=>{
						if(item.id==this.readCondition.readType){
							this.readModuleMenu=index;
						}
					})
					uni.removeStorageSync("readType");
				}
				if(uni.getStorageSync("readTypeSub")!=''){
					this.readCondition.readTypeSub=parseInt(uni.getStorageSync("readTypeSub"));
					this.readSubModuleMenuTexts.forEach((item,index)=>{
						if(item.id==this.readCondition.readTypeSub){
							this.readSubModuleMenu=index;
						}
					})
					uni.removeStorageSync("readTypeSub");
				}
				if(uni.getStorageSync("readCurrent")!=''){
					this.readCondition.current=parseInt(uni.getStorageSync("readCurrent"));
					uni.removeStorageSync("readCurrent");
				}
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminRead/getRead",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.readCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.reads = data.data.data;
															
								this.readCondition.current =data.data.current;
								this.readCondition.total=data.data.total;
								this.readCondition.size=data.data.pageSize;
								this.reads.forEach((item,index)=>{
									item.image = this.globalData.url+"static/image/readImage/"+item.image;
								});
								console.log(this.reads);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载文章 end
			//加载录音文件是否存在 start
			loadWordAudioFileExists(word,type){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminFile/getWordAudioFileExist",
							method:'POST',
							header:this.globalData.header.normal,
							dataType:"json",
							data:{
								word:word,
								type:type
							}
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								resolve(true);
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {								
								resolve(false);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载录音文件是否存在 end
			//加载单词 start
			loadWords(){
				if(uni.getStorageSync("initial")!=''){
					this.wordCondition.initial=uni.getStorageSync("initial");
					this.wordModuleMenuTexts.forEach((item,index)=>{
						if(item.text.toLowerCase()==this.wordCondition.initial.toLowerCase()){
							this.wordModuleMenu=index;
						}
					})
					uni.removeStorageSync("initial");
				}
				if(uni.getStorageSync("wordCurrent")!=''){
					this.wordCondition.current=parseInt(uni.getStorageSync("wordCurrent"));
					uni.removeStorageSync("wordCurrent");
				}
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminWord/getAllWordsByInitial",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.wordCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.words = data.data.data;
															
								this.wordCondition.current =data.data.current;
								this.wordCondition.total=data.data.total;
								this.wordCondition.size=data.data.pageSize;
								this.words.forEach((item,index)=>{
									item.liju.sentences =this.globalData.Base64.decode(item.liju.sentences);  
									item.liju.sentences.forEach((item2,index2)=>{
										if(item2!=undefined&&item2.english!=undefined){
											item2.split = item2.english.split(" ");
											let wordSplit = item.word.word.split(" ");
											item2.styleIndexStart =this.globalData.matchingAtr(item2.split,wordSplit);
											item2.styleIndexEnd =item2.styleIndexStart+ wordSplit.length;
										}
										
										
									});
									this.loadWordAudioFileExists(item.word.word,1).then((res2)=>{
										item.audioUKFileExist = res2;
										this.loadWordAudioFileExists(item.word.word,2).then((res2)=>{
											item.audioUSAFileExist = res2;
										});
									})
								});
								console.log(this.words);
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载单词 end
			//加载单词首字母 start
			loadWordInitials(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminWord/choosePick",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.wordModuleMenuTexts=data.data;
								this.wordModuleMenuTexts=this.wordModuleMenuTexts.concat();
							
								
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载单词首字母 end
			//加载管理员 start 供筛选用
			loadAdmin(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminUser/getAllAdminNoPage",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.cikuUserFilterData=[];
								data.data.forEach((item)=>{
									this.cikuUserFilterData.push({text:'管理员：'+item.username,value:item.id});
								})
							
								
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载管理员 end 供筛选用
			
			//加载词库 start
			loadCikus(){
				if(uni.getStorageSync("cikuTypeId")!=''){
					this.cikuCondition.cikuTypeId=parseInt(uni.getStorageSync("cikuTypeId"));
					uni.removeStorageSync("cikuTypeId");
				}
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminCiku/getAllCikus",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.cikuCondition
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.cikus = data.data.data;
															
								this.cikuCondition.current =data.data.current;
								this.cikuCondition.total=data.data.total;
								this.cikuCondition.size=data.data.pageSize;
								this.cikus.forEach(item=>{
									item.user.headimage=this.globalData.url+"static/image/headImage/"+item.user.headimage;
								})
								
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载词库 end
			
			//加载词库type start
			loadCikuTypes(){
				return new Promise((resolve,reject)=>{
						uni.request({
							url:this.globalData.url+"adminCiku/getAllCikuTypes",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.cikuModuleMenuTexts = data.data;
							
								this.cikuCondition.cikuTypeId=this.cikuModuleMenuTexts[0].id;
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})		
				})
			},
			//加载词库type end
			
			//加载主页设置 start
			loadMainSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"static/setting/wordsetting.json",
						method:'GET',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						console.log(res);
						
							res.data.data.everyDayWordModule.baseEveryDayWordSize.value=parseInt(res.data.data.everyDayWordModule.baseEveryDayWordSize.value);
							res.data.data.everyDayWordModule.baseEveryDayWordFactor.value=parseInt(res.data.data.everyDayWordModule.baseEveryDayWordFactor.value);
							res.data.data.everyDayWordModule.baseEveryDayWordMutipiNum.value=parseInt(res.data.data.everyDayWordModule.baseEveryDayWordMutipiNum.value);
							this.indexSetting=res.data;
							this.indexSettingBackUp = new Object();
							this.indexSettingBackUp=JSON.parse(JSON.stringify(res.data));
							
							console.log(this.indexSetting);
							resolve();
						
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载主页设置 end
			
			//加载用户 start
			loadUsers(){
				return new Promise((resolve,reject)=>{
					if(this.userModuleMenu==0){
						uni.request({
							url:this.globalData.url+"adminUser/getAllUser",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.userCondition,
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.users = data.data.data;
							
								this.userCondition.current =data.data.current;
								this.userCondition.total=data.data.total;
								this.userCondition.size=data.data.pageSize;
								this.users.forEach(item=>{
									item.headimage=this.globalData.url+"static/image/headImage/"+item.headimage;
								})
								
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})
					}else{
						uni.request({
							url:this.globalData.url+"adminUser/getAllAdmin",
							method:'POST',
							header:this.globalData.header.json,
							dataType:"json",
							data:this.userCondition,
						}).then((res)=>{
							let data =res.data;
							if (data.code == 200) {
								// console.log(this.globalData.Base64.decode( data.data.sentences));
								this.users = data.data.data;
								this.userCondition.current =data.data.current;
								this.userCondition.total=data.data.total;
								this.userCondition.size=data.data.pageSize;
								this.users.forEach(item=>{
									item.headimage=this.globalData.url+"static/image/headImage/"+item.headimage;
								})
								
								resolve();
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
								resolve(data.message);;
							} else {
								console.log(data.message);
								resolve(data.message);
							}
						}).catch((e)=>{
							console.log(e);
							reject(e);
						})
					}
		
				})
			},
			//加载用户 end
			
				
			//加载用户基本设置 start
			loadUserBasicSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminUserSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.userBasicSetting = data.data;
							if(this.userBasicSetting.lastRegisters!=null) {
								this.userBasicSetting.lastRegisters.forEach(item=>{
									item.headimage=this.globalData.url+"static/image/headImage/"+item.headimage;
								})
							}
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载用户基本设置 end
			//加载词库管理基本设置 start
			loadBasicCikuSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminCikuSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.cikuBasicSetting = data.data;
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载词库管理基本设置 end
			//加载单词管理基本设置 start
			loadBasicWordSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminWordSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.wordBasicSetting = data.data;
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载单词基本管理设置 end
			//加载阅读管理基本设置 start
			loadBasicReadSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminReadSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.readBasicSetting = data.data;
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载阅读管理基本设置 end
			//加载题库管理基本设置 start
			loadBasicTestSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminTestSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.testBasicSetting = data.data;
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载题库管理基本设置 end
			//加载积分管理基本设置 start
			loadBasicScoreSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminScoreSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.scoreBasicSetting = data.data;
							if(this.scoreBasicSetting.max!=null&&this.scoreBasicSetting.max.user!=null){
								this.scoreBasicSetting.max.user.headimage= this.globalData.url+"static/image/headImage/"+this.scoreBasicSetting.max.user.headimage;
							}
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载积分管理基本设置 end
			//加载商品管理基本设置 start
			loadBasicProductSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminProductSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.productBasicSetting = data.data;
							if(this.productBasicSetting.nodes!=null){
								this.productBasicSetting.nodes.forEach((item,index)=>{
									if(item.merchant!=null){
										this.productBasicSetting.nodes[index].merchant.headimage=this.globalData.url+"static/image/headImage/"+this.productBasicSetting.nodes[index].merchant.headimage;
									}
									
								})
							}
							console.log(this.productBasicSetting);
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载商品管理基本设置 end
			//加载订单管理基本设置 start
			loadBasicOrderSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminOrderSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.orderBasicSetting = data.data;
							
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载订单管理基本设置 end
			//加载数据管理基本设置 start
			loadBasicDataSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminDataSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.dataBasicSetting = data.data;
							
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载数据管理基本设置 end
			//加载学习圈管理基本设置 start
			loadBasicStudyCircleSetting(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminStudyCircleSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							this.studyCircleBasicSetting = data.data;
							console.log(this.studyCircleBasicSetting);
							if(this.studyCircleBasicSetting.lastStudyCircles!=null){
								this.studyCircleBasicSetting.lastStudyCircles.forEach((item,index)=>{
									item.user.headimage= this.globalData.url+"static/image/headImage/"+item.user.headimage;
								})
							}
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
			//加载学习圈基本管理设置 end
			//加载数据 end
		
			//加载数据 start
			loadData(){
				this.loadNetData()
			},
			//加载数据 end
			//加载服务器数据 start
			loadNetData(){
				this.initMenu(this.menu);
			},
			//加载服务器数据 end
			loadExample(){
				return new Promise((resolve,reject)=>{
					uni.request({
						url:this.globalData.url+"adminUserSetting/getBasicSetting",
						method:'POST',
						header:this.globalData.header.normal,
						dataType:"json",
					}).then((res)=>{
						let data =res.data;
						if (data.code == 200) {
							// console.log(this.globalData.Base64.decode( data.data.sentences));
							resolve();
						} else if (data.code == 520) {
							this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							resolve(data.message);;
						} else {
							console.log(data.message);
							resolve(data.message);
						}
					}).catch((e)=>{
						console.log(e);
						reject(e);
					})
				})
			},
		}
	}
</script>

<style>
	.titles1 {
		word-wrap: break-word;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-inline-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1; 
		/*超出2行就显示省略号，可以填3或者其它正整数*/
	}
	.titles2 {
		word-wrap: break-word;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-inline-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2; 
		/*超出2行就显示省略号，可以填3或者其它正整数*/
	}
	.baseColum{
		width: 30%;
		height: 37vh;
		margin-left: 50rpx;
		border: 2rpx solid grey;
		margin-bottom: 30rpx;
		border-radius: 20rpx;
		margin-top: 20rpx;
	}
	.content{
		padding: 20rpx;
	}
	.scrollXTarBarContent {
		display: inline-block;
		height: 50rpx;
		line-height: 50rpx;
		text-align: center;
	
		
		/* background-color: beige; */
		padding: 30rpx;
	}
	.scrollXTarBarText {
		padding-bottom: 10rpx;
		display: inline-block;
		color: #d5d5d5;
		font-size: 33rpx;
	}
	.activeText {
		color: rgb(254, 254, 254);
		border-bottom: 4px solid rgb(255, 255, 255);
		font-weight: bolder;
		font-size: 36rpx;
	}
	.scrollXTarBar {
		white-space: nowrap;
		width: 100%;
	
		border-bottom: 1px solid rgb(239, 239, 239)
	}
 .header{

}
.body{

}
.content{

}
.footer{

}

</style>
