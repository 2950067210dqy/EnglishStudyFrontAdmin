<script>
	export default {
		globalData: {
				unit: 'rpx',
			webUrl:'http://localhost:5174/#/pages/word/word',
			youdaoUrlBase:'https://dict.youdao.com/',
			youdaoUrl:'https://dict.youdao.com/result?lang=en&word=',
			// webUrl:'http://114.119.185.255:8085/#/pages/word/word',
			url: 'http://192.168.0.114:8086/',
			// url:"https://www.dengqinyou.cn/",
			mediaUrl: 'http://192.168.0.114:8086/static/',
			// mediaUrl:'https://www.dengqinyou.cn/static/',
			audioSentenceUrl:"http://dict.youdao.com/dictvoice?",
			// defaultHeadImage:'https://www.dengqinyou.cn/static/image/defaultHeadImage/1.png',
			defaultHeadImage:'http://192.168.0.114:8086/static/image/defaultHeadImage/1.png',
			// backgroundUrl1:'https://www.dengqinyou.cn/static/image/background/bg1.jpg',
			backgroundUrl1:'http://192.168.0.114:8086/static/image/background/bg1.jpg',
			// backgroundUrl2:'https://www.dengqinyou.cn/static/image/background/bg2.jpg',
			backgroundUrl2:'http://192.168.0.114:8086/static/image/background/bg2.jpg',
			// backgroundUrl3:'https://www.dengqinyou.cn/static/image/background/bg3.jpg',
			backgroundUrl3:'http://192.168.0.114:8086/static/image/background/bg3.jpg',
				contentType: {
					normal: { 'Content-Type': 'application/x-www-form-urlencoded' },
					json: { 'Content-Type': 'application/json' },
					media: { 'Content-Type': 'multipart/form-data' },
					normalData: 'application/x-www-form-urlencoded',
					jsonData: 'application/json',
					mediaData: 'multipart/form-data'
					// cloud:{'Content-Type':'application/json', "X-WX-SERVICE": "help"},
					// cloud_normal:{'Content-Type':'application/x-www-form-urlencoded', "X-WX-SERVICE": "help"} ,
					// cloud_media:{'Content-Type':'multipart/form-data', "X-WX-SERVICE": "help"}
				},
				header:{
					json:{
						'content-type':'application/json',
						'token':uni.getStorageSync('token')
					},
					normal:{
						'content-type':'application/x-www-form-urlencoded',
						'token':uni.getStorageSync('token')
					},
					media:{
						'content-type':'multipart/form-data',
						'token':uni.getStorageSync('token')
					}
				},
				maxHeight: 0,
				maxWidth: 0,
				learnNum:1,
				reviewNum:2,
				testNum:2,
				testAllScore:100,//考试总分
				testExamTime:120,//考试时间
				//字符串匹配 朴素匹配 start
				matchingAtr:function(arr, sunArr) {
							let i = 0, j = 0;//i表示arr匹配的元素位置，j为sunArr匹配的元素位置
							while(i < arr.length && j < sunArr.length) {
							
								// 
								if(arr[i].toLowerCase().includes(sunArr[j].toLowerCase()) ) {
									i ++;
									j ++;
								} else {
									i = i - j + 1;//因为要下一位，所以+1
									j = 0;
								}
							}
							if(j == sunArr.length) {
								return i - j;
							} else {
								return -1;
							}
				},
				//字符串匹配 朴素匹配 end
				//校验token是否过期 start
				checkTokenTime:function(url,cotentType){
					uni.request({
						url:url+"user/checkToken",
						method:'POST',
						dataType:'json',
						header:{
							'content-type':cotentType,
							'token':uni.getStorageSync("token")
						},
						success: (res) => {
							let data= res.data;
							if(data.code==520){
								uni.clearStorage();
								uni.showModal({
									title: '提示',
									content: '登录已过期,请重新登录',
									showCancel: false,
									success: res => {
										if (res.confirm) {
											uni.navigateTo({
												url: '/pages/login/login'
											});
										}
									}
								});
							}
						},
						fail: (e) => {
							uni.showModal({
								title: '提示',
								content: '登录已过期,请重新登录',
								showCancel: false,
								success: res => {
									if (res.confirm) {
										uni.navigateTo({
											url: '/pages/login/login'
										});
									}
								}
							});
						}
						
					})
				},
				//检验token是否过期 end
				checkLogin: function() {
					let token = uni.getStorageSync('token');
					console.log('token', token);
					console.log('token', typeof token);
					if (token.trim() == '') {
						uni.hideToast();
				
						uni.showModal({
							title: '提示',
							content: '未登录,请登录',
							showCancel: false,
							success: res => {
								if (res.confirm) {
									uni.navigateTo({
										url: '/pages/login/login'
									});
								}
							},fail: (e) => {
								console.log(e);
							}
						});
						// uni.showLoading({
						// 	title:"正在跳转"
						// })
						// setTimeout(function(){
						// 	uni.hideLoading();
						// 	uni.navigateTo({
						// 		url:"/pages/login/login"
						// 	})
						// },1000)
						return false;
					}else{
						return true;
					}
				},
				getDifferTime(firstdate,seconddate){
					 // 1.对事件进行处理
					 console.log("firstdate",firstdate);
					  console.log("seconddate",seconddate);
					      var firsttime = Date.parse(firstdate );
					      var secondtime = Date.parse(seconddate);
						  		  console.log("firsttime",firsttime,typeof firsttime);
								  		  console.log("secondtime",secondtime,typeof secondtime);
					      // 2.获取时间间隔
					      var timespan = secondtime - firsttime;
						  console.log("timespan",timespan);
					      // 3.获取天
					      var day = Math.floor(timespan / 1000 / 60 / 60 / 24);
					     // 4.获取小时
					    var hour = Math.floor(timespan / 1000 / 60 / 60);
					     // 5.获取分钟
					     var minute = Math.floor(timespan / 1000 / 60 - hour * 60);
					     // 6.获取秒
					     var second = timespan / 1000 - hour * 60 * 60 - minute * 60;
					     return {
							 day:day,
							 hour:hour,
							 min:minute,
							 sec:second
						 };
				},
				getDateDiff(dateTimeStamp) {
						var minute = 1000 * 60;
						var hour = minute * 60;
						var day = hour * 24;
						var halfamonth = day * 15;
						var month = day * 30;
						var now = new Date().getTime();
						var diffValue = now - Date.parse(dateTimeStamp) ;
						if (diffValue < 0) {
							return;
						}
						var monthC = diffValue / month;
						var weekC = diffValue / (7 * day);
						var dayC = diffValue / day;
						var hourC = diffValue / hour;
						var minC = diffValue / minute;
						var result = '';
						// console.log(monthC,weekC,dayC,hourC,minC);
						if (monthC >= 1) {
							// result = "" + parseInt(monthC) + "月前";
							result = dateTimeStamp.replace("T"," ");
						} else if (weekC >= 1) {
							// result = "" + parseInt(weekC) + "周前";
							result =  dateTimeStamp.replace("T"," ");
						} else if (dayC >= 1) {
							// result = "" + parseInt(dayC) + "天前";
							result = dateTimeStamp.replace("T"," ");
						} else if (hourC >= 1) {
							result = "" + parseInt(hourC) + "小时前";
						} else if (minC >= 1) {
							result = "" + parseInt(minC) + "分钟前";
						} else
							result = "刚刚";
						return result;
					},
				/**
					 * 获取任意时间
					 */
				 getDate(date, AddDayCount = 0) {
						if (!date) {
							date = new Date()
						}
						if (typeof date !== 'object') {
							date = date.replace(/-/g, '/')
						}
						const dd = new Date(date)
				
						dd.setDate(dd.getDate() + AddDayCount) // 获取AddDayCount天后的日期
				
						const y = dd.getFullYear()
						const m = dd.getMonth() + 1 < 10 ? '0' + (dd.getMonth() + 1) : dd.getMonth() + 1 // 获取当前月份的日期，不足10补0
						const d = dd.getDate() < 10 ? '0' + dd.getDate() : dd.getDate() // 获取当前几号，不足10补0
						return {
							fullDate: y + '-' + m + '-' + d,
							year: y,
							month: m,
							date: d,
							day: dd.getDay()
						}
					},
				getDateTime(flag = 0, weekType = 0) {
					let date = new Date();
					let year = date.getFullYear(); // 年
					let month = date.getMonth() + 1; // 月
					let day = date.getDate(); // 日
					let week = date.getDay(); // 星期
					let weekArr = [];
					if (weekType == 0) {
						weekArr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
					} else {
						weekArr = ['Sun.', 'Mon.', 'Tue.', 'Wed.', 'Thu.', 'Fri.', 'Sat.'];
					}
		
					let hour = date.getHours(); // 时
					hour = hour < 10 ? '0' + hour : hour; // 如果只有一位，则前面补零
					let minute = date.getMinutes(); // 分
					minute = minute < 10 ? '0' + minute : minute; // 如果只有一位，则前面补零
					let second = date.getSeconds(); // 秒
					second = second < 10 ? '0' + second : second; // 如果只有一位，则前面补零
					let result = '';
					switch (flag) {
						case 0: //获取所有
							result = `${year}/${month}/${day} ${hour}:${minute}:${second} ${weekArr[week]}`;
							break;
						case 1: //不带星期几
							result = `${year}/${month}/${day} ${hour}:${minute}:${second}`;
							break;
						case 2: //只有年月日 没有时间
							result = `${year}/${month}/${day}`;
						case 3: //只有年月日 没有时间 有星期
							result = `${year}/${month}/${day}   ${weekArr[week]}`;
							break;
						case 4: //只有时间 没有年月日
							result = `${hour}:${minute}:${second}`;
							break;
						case 5: //只有时间 没有年月日 有星期
							result = `${hour}:${minute}:${second}    ${weekArr[week]}`;
							break;
						case 6: //只有年月
							result = `${year}/${month}`;
							break;
						case 7: //只有年月 和星期
							result = `${year}/${month}  ${weekArr[week]}`;
							break;
						case 8: //只有月日
							result = `${month}/${day}`;
							break;
						case 9: //只有月日 和星期
							result = `${month}/${day}  ${weekArr[week]}`;
							break;
						case 10: //只有时间 且只有时分
							result = `${hour}:${minute}`;
						case 11: //只有时间 且只有时分 和星期
							result = `${hour}:${minute}  ${weekArr[week]}`;
						case 12: //只有年
							result = `${year}`;
							break;
						case 13: //只有月
							result = `${month}`;
							break;
						case 14: //只有日
							result = `${day}`;
							break;
						case 15: //只有小时
							result = `${hour}`;
							break;
						case 16: //只有分钟
							result = `${minute}`;
							break;
						case 17: //只有秒数
							result = `${second}`;
							break;
						case 18: //只有星期几
							result = `${weekArr[week]}`;
							break;
						case 19: //只有星期几
							result = `${year}-${month}-${day}`;
							break;
						case 20:
							result = year;
							break;
						default:
							result = `日期参数格式错误`;
							break;
					}
					return result;
				},
				//随机数 几位 每位最大多少
				done(len, max) {
					var arr = [];
					var result = '';
					var count = 0;
					while (count < len) {
						var n = Math.floor(Math.random() * max + 1);
						if (arr.join().indexOf(n) == -1) {
							arr.push(n);
							count++;
						}
					}
					for (let index = 0; index < arr.length; index++) {
						result = result + arr[index];
					}
					return result;
				},
			
				Base64: {
					// private property
					_keyStr: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=',
					// public method for encoding
					encode: function(input) {
						var output = '';
						var chr1, chr2, chr3, enc1, enc2, enc3, enc4;
						var i = 0;
						// input =this. _checkChar(input)
						input = this._utf8_encode(input);
						// console.log(input);
						while (i < input.length) {
							chr1 = input.charCodeAt(i++);
							chr2 = input.charCodeAt(i++);
							chr3 = input.charCodeAt(i++);
							enc1 = chr1 >> 2;
							enc2 = ((chr1 & 3) << 4) | (chr2 >> 4);
							enc3 = ((chr2 & 15) << 2) | (chr3 >> 6);
							enc4 = chr3 & 63;
							if (isNaN(chr2)) {
								enc3 = enc4 = 64;
							} else if (isNaN(chr3)) {
								enc4 = 64;
							}
							output = output + this._keyStr.charAt(enc1) + this._keyStr.charAt(enc2) + this._keyStr.charAt(enc3) + this._keyStr.charAt(enc4);
						}
						// output = output.replace('+', '%2B'); // 这个后台识别不了+，所以替换了一下，这个也可以不用替换，视实际情况定
						return output;
					},
		
					// public method for decoding
					decode: function(input,type=0) {
						var output = '';
						var chr1, chr2, chr3;
						var enc1, enc2, enc3, enc4;
						var i = 0;
						if (typeof input == 'string') {
							input = input.replace(/[^A-Za-z0-9\+\/\=]/g, '');
						} else {
							return input;
						}
						while (i < input.length) {
							enc1 = this._keyStr.indexOf(input.charAt(i++));
							enc2 = this._keyStr.indexOf(input.charAt(i++));
							enc3 = this._keyStr.indexOf(input.charAt(i++));
							enc4 = this._keyStr.indexOf(input.charAt(i++));
							chr1 = (enc1 << 2) | (enc2 >> 4);
							chr2 = ((enc2 & 15) << 4) | (enc3 >> 2);
							chr3 = ((enc3 & 3) << 6) | enc4;
							output = output + String.fromCharCode(chr1);
							if (enc3 != 64) {
								output = output + String.fromCharCode(chr2);
							}
							if (enc4 != 64) {
								output = output + String.fromCharCode(chr3);
							}
						}
						output = this._utf8_decode(output);
						// console.log(output);
						output = this._returnJson(output,type);
						return output;
					},
		
					// private method for UTF-8 encoding
					_utf8_encode: function(string) {
						string = string.replace(/\r\n/g, '\n');
						var utftext = '';
						for (var n = 0; n < string.length; n++) {
							var c = string.charCodeAt(n);
							if (c < 128) {
								utftext += String.fromCharCode(c);
							} else if (c > 127 && c < 2048) {
								utftext += String.fromCharCode((c >> 6) | 192);
								utftext += String.fromCharCode((c & 63) | 128);
							} else {
								utftext += String.fromCharCode((c >> 12) | 224);
								utftext += String.fromCharCode(((c >> 6) & 63) | 128);
								utftext += String.fromCharCode((c & 63) | 128);
							}
						}
						// console.log(utftext);
						return utftext;
					},
		
					// private method for UTF-8 decoding
					_utf8_decode: function(utftext) {
						var string = '';
						var i = 0;
						var c1, c2, c3;
						var c = (c1 = c2 = 0);
						while (i < utftext.length) {
							c = utftext.charCodeAt(i);
							if (c < 128) {
								string += String.fromCharCode(c);
								i++;
							} else if (c > 191 && c < 224) {
								c2 = utftext.charCodeAt(i + 1);
								string += String.fromCharCode(((c & 31) << 6) | (c2 & 63));
								i += 2;
							} else {
								c2 = utftext.charCodeAt(i + 1);
								c3 = utftext.charCodeAt(i + 2);
								string += String.fromCharCode(((c & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63));
								i += 3;
							}
						}
						return string;
					},
					_checkChar: function(textJson) {
						var pattern = new RegExp('[\u4E00-\u9FA5]+');
						for (let item in textJson) {
							if (pattern.test(textJson[item])) {
								textJson[item] = encodeURIComponent(textJson[item]);
							}
						}
						return JSON.stringify(textJson);
					},
					_returnJson: function(textString,type) {
						let res;
						if(type==0){
							res = JSON.parse(textString);
						}else{
							res=textString;
						}
						
						// for (let item in res) {
						// 	res[item] = decodeURIComponent(res[item]);
						// }
						return res;
					}
				},
				
			},
			methods: {	//随机数 几位 每位最大多少
				done(len, max) {
					var arr = [];
					var result = '';
					var count = 0;
					while (count < len) {
						var n = Math.floor(Math.random() * max + 1);
						if (arr.join().indexOf(n) == -1) {
							arr.push(n);
							count++;
						}
					}
					for (let index = 0; index < arr.length; index++) {
						result = result + arr[index];
					}
					return result;
				},
		},
		
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
