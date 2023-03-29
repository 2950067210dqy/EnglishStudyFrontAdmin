<template>
	<view style="background: white;">
		<my-navBack backColor="white"  background="linear-gradient(90deg, #0d0d0d, #595959)" :title="this.cikuId==null?'添加词库':'修改词库'"></my-navBack>
		<view style="padding: 30rpx;">
			<uni-forms validate-trigger="bind" ref="importCikuForm" :modelValue="formData" labelWidth="150">
				<!-- 	<uni-forms-item   name="cikuTypeId">
						<uni-easyinput   type="hidden" v-model="formData.selectCikuType.cikuTypeId"  />
					</uni-forms-item> -->
				<uni-forms-item required label="请选择词库类别:" name="cikuType">
					<uni-easyinput
						:trim="true"
						type="text"
						disabled
						v-model="cikuType.dsc"
					></uni-easyinput>
				</uni-forms-item>
				<uni-forms-item required label="请输入词库名称:" name="cikuName">
					<uni-easyinput
						:trim="true"
						@blur="binddata('cikuName', formData.cikuName, 'importCikuForm')"
						type="text"
						v-model="formData.cikuName"
						placeholder="请输入词库名称"
					/>
				</uni-forms-item>
				<uni-forms-item required label="请输入词库简写名称:" name="cikuNameABB">
					<uni-easyinput
						:trim="true"
						@blur="binddata('cikuNameABB', formData.cikuNameABB, 'importCikuForm')"
						type="text"
						v-model="formData.cikuNameABB"
						placeholder="请输入词库简写名称(最大15个字符)"
					/>
				</uni-forms-item>

				<uni-forms-item v-if="this.cikuId==null" required label="请选择词库文件(json,txt):" name="cikuFile">
					<uni-file-picker
						@blur="binddata('cikuFile', formData.importCikuFileValue, 'importCikuForm')"
						@select="cikuFileSelect"
						@success="cikuFileSucess"
						@fail="cikuFileFail"
						@delete="cikuFileDelete"
						@progress="cikuFileProgress"
						ref="importFileByCiku"
						limit="1"
						file-mediatype="all"
						title="最多选择1个文件"
						fileExtname="json,txt"
						v-model="formData.importCikuFileNameList"
					></uni-file-picker>
				</uni-forms-item>
			</uni-forms>
			<button v-if="this.cikuId==null" type="primary" @click="importSubmit">确定提交</button>
			<button v-else type="primary" @click="updateSubmit">确定修改</button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			globalData: {},
			formData: {
				cikuTypeId:0,
				cikuName: '',
				cikuNameABB: '',
				cikuId:0,
				importCikuFileValue: {},
				importCikuFileNameList: []
			},
			cikuId:null,
			ciku:{},
			cikuType:{
				id:0,
				dsc:'',
			},
			importCikuFormRules: {
				//cikuName 字段检验规则
				cikuName: {
					label: '词库名',
					rules: [
						{
							required: true,
							errorMessage: '{label}不能为空，请填写'
						},
						// 对name字段进行长度验证
						{
							minLength: 2,
							maxLength: 30,
							errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
						},
						{
							format: 'string',
							errorMessage: '{label}必须为字符串'
						},
						{
							validateFunction: (rule, value, data, callback) => {
								// 异步需要返回 Promise 对象
								return new Promise((resolve, reject) => {
								
										// console.log(data);
										uni.request({
											url: this.globalData.url + 'ciku/selectByDsc',
											method: 'POST',
											dataType: 'json',
											header: {
												'content-type': this.globalData.contentType.normalData,
												token: uni.getStorageSync('token')
											},
											data: {
												cikuTypeId: this.formData.cikutypeId,
												dsc: value
											},
											success: res => {
												let data = res.data;
												if (data.code == 200) {
													reject(new Error(data.message));
												} else if (data.code == 520) {
													this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
												} else {
													resolve();
												}
											},
											fail: error => {
												reject(new Error('请求数据错误'));
											}
										});
									
								});
							}
						}
					]
				},
				cikuNameABB: {
					label: '词库名简写',
					rules: [
						{
							required: true,
							errorMessage: '{label}不能为空，请填写'
						},
						// 对name字段进行长度验证
						{
							minLength: 1,
							maxLength: 50,
							errorMessage: '{label}长度在 {minLength} 到 {maxLength} 个字符'
						},
						{
							format: 'string',
							errorMessage: '{label}必须为字符串'
						},
						{
							validateFunction: function(rule, value, data, callback) {}
						}
					]
				},
				cikuFile: {
					label: '词库文件',
					rules: [
						{
							required: true,
							errorMessage: '{label}不能为空，请上传'
						},
						{
							validateFunction: function(rule, value, data, callback) {
								// // return false;
								// console.log(value);
								// if (data === {}) {
								// 	callback('{label}不能为空，请上传');
								// } else {
								// 	callback();
								// }
							}
						}
					]
				}
			}
		};
	},
	onLoad(param) {
		this.globalData = getApp().globalData;
		this.cikuType.id =parseInt(param.cikutypeId); 
		if(param.cikuId!=undefined&&param.cikuId!=0){
			this.cikuId=parseInt(param.cikuId);
		}
		console.log("cikuid",this.cikuId);
		if (this.globalData.checkLogin()) {
			this.loadData();
		}
	},
	onShow() {
		this.globalData = getApp().globalData;
		uni.setStorageSync("cikuTypeId",this.cikuType.id);
		if(this.cikuId!=null){
			this.loadCiku();
		}
	},
	onReady() {
		// 需要在onReady中设置规则
	
			this.$refs['importCikuForm'].setRules(this.importCikuFormRules);
		
		
	},
	methods: {
		//加载数据 start
		loadData() {
			this.loadCikuTypes();
		},
		//加载数据 end
		//获取词库 start
		loadCiku(){
			uni.showLoading({
				title: '加载数据'
			});
			uni.request({
				url: this.globalData.url + 'ciku/selectById',
				method: 'POST',
				dataType: 'json',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data: {
					cikuTypeId:this.cikuType.id,
					cikuId:this.cikuId
				},
				success: res => {
					// 此处的 res 参数，与使用默认				方式调用时 success 回调中的 res 参数一致
					uni.hideLoading();
					let data = res.data;
					if (data.code == 200) {
						this.ciku=data.data;
						this.formData.cikuName=data.data.dsc;
						this.formData.cikuNameABB=data.data.dscabb;
						this.formData.cikuId=data.data.id;
					} else if (data.code == 520) {
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.showToast({
							title: '获取失败1',
							icon: 'error'
						});
					}
				},
				fail: err => {
					uni.hideLoading();
					uni.showToast({
						title: '获取失败2',
						icon: 'error'
					});
				}
			});
		},
		//获取词库 end
		//获取cikutype start
		loadCikuTypes() {
			uni.showLoading({
				title: '加载数据'
			});
			uni.request({
				url: this.globalData.url + 'cikutype/get',
				method: 'POST',
				dataType: 'json',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data: {
					id:this.cikuType.id
				},
				success: res => {
					// 此处的 res 参数，与使用默认				方式调用时 success 回调中的 res 参数一致
					uni.hideLoading();
					let data = res.data;
					if (data.code == 200) {
						
						
						this.formData.cikutypeId=data.data.id;
						this.cikuType.dsc = data.data.dsc;
					} else if (data.code == 520) {
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.showToast({
							title: '获取失败1',
							icon: 'error'
						});
					}
				},
				fail: err => {
					uni.hideLoading();
					uni.showToast({
						title: '获取失败2',
						icon: 'error'
					});
				}
			});
		},
		//获取cikutype end
		//词库文件选择文件触发的方法 start
		cikuFileSelect(e) {
			console.log('选择文件', e);
			// e.tempFilePaths.forEach((item,index)=>{
			// 	this.formData.importCikuFileValue.tempFilePaths.push(item);
			// 	return item;
			// });
			// e.tempFiles.forEach((item,index)=>{
			// 	this.formData.importCikuFileValue.tempFiles.push(item);
			// 	return item;
			// });
			this.formData.importCikuFileValue = e;
			// console.log('选择文件2', this.formData.importCikuFileValue);

			//为uniform 表单添加校验该字段的数据
			this.$refs['importCikuForm'].setValue('cikuFile', this.formData.importCikuFileValue);
		},
		//词库文件选择文件触发的方法 end
		//词库文件上传文件成功触发的方法 start
		cikuFileSucess(e) {
			console.log('上传文件成功', e);
		},
		//词库文件上传文件成功触发的方法 end
		//词库文件上传文件成功失败的方法 start
		cikuFileFail(e) {
			console.log('上传文件失败', e);
		},
		//词库文件上传文件成功失败的方法 end
		//词库文件列表移除时触发的方法 start
		cikuFileDelete(e) {
			console.log('删除文件从列表', e);
			this.formData.importCikuFileValue = {};
			//为uniform 表单添加校验该字段的数据
			this.$refs['importCikuForm'].setValue('cikuFile', this.formData.importCikuFileValue);
			// console.log('删除文件从列表2',this.formData.importCikuFileValue);
		},
		//词库文件列表移除时触发的方法 end
		//词库文件上传时触发的方法 start
		cikuFileProgress(e) {
			console.log('上传文件进度', e);
		},
		//词库文件上传时触发的方法 end
		//重置输入的数据为空 start
		resetFormData() {
			this.formData.cikuName = '';
			this.formData.cikuNameABB = '';
			this.formData.cikuId=0;
			this.formData.importCikuFileValue = {};
			this.formData.importCikuFileNameList = [];
			this.cikuFileDelete();
		},
		//重置输入的数据为空 end
		//修改词库 start
		updateSubmit(){
			uni.showLoading({
				title: '修改数据'
			});
			uni.request({
				url: this.globalData.url + 'adminCiku/updateCiku',
				method: 'POST',
				dataType: 'json',
				header: {
					'content-type': this.globalData.contentType.normalData,
					token: uni.getStorageSync('token')
				},
				data: {
					cikuTypeId:this.cikuType.id,
					cikuId:this.cikuId,
					dsc:this.formData.cikuName,
					dscabb:this.formData.cikuNameABB,
					uid:JSON.parse(uni.getStorageSync("user")).id 
				},
				success: res => {
					// 此处的 res 参数，与使用默认				方式调用时 success 回调中的 res 参数一致
					uni.hideLoading();
					let data = res.data;
					if (data.code == 200) {
						uni.showToast({
							title: '修改成功',
						
						});
						setTimeout(()=>{
							uni.navigateBack();
						},500)
						
					} else if (data.code == 520) {
						this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
					} else {
						uni.showToast({
							title: '修改失败1',
							icon: 'error'
						});
					}
				},
				fail: err => {
					uni.hideLoading();
					uni.showToast({
						title: '修改失败2',
						icon: 'error'
					});
				}
			});
		},
		//修改词库 end
		//提交按钮触发事件 start
		importSubmit(e) {
			this.$refs['importCikuForm']
				.validate()
				.then(res => {
					console.log('表单数据信息：', res);
					uni.showLoading({
						title: '正在提交中'
					});
					// 使用 Promise then/catch 方式调用
					uni.uploadFile({
						url: this.globalData.url + 'file/uploadTxt',
						filePath: this.formData.importCikuFileValue.tempFilePaths[0],
						name: 'file',
						header: {
							token: uni.getStorageSync('token')
						},
						formData: {
							cikuType: this.cikuType.dsc,
							cikuTypeId: this.formData.cikutypeId,
							cikuName: this.formData.cikuName,
							cikuNameABB: this.formData.cikuNameABB,
							uid:JSON.parse(uni.getStorageSync("user")).id
						}
					})
						.then(res => {
							// 此处的 res 参数，与使用默认方式调用时 success 回调中的 res 参数一致
							// console.log(res.data);
							uni.hideLoading();
							let data = JSON.parse(res.data);
							if (data.code == 200) {
								uni.showToast({
									title: '提交成功'
								});
								setTimeout(()=>{
									uni.navigateBack();
								},500)
							} else if (data.code == 520) {
								this.globalData.checkTokenTime(this.globalData.url, this.globalData.contentType.jsonData);
							} else {
								uni.showToast({
									title: data.message,
									icon: 'error'
								});
							}

							// this.resetFormData();
						})
						.catch(err => {
							// 此处的 err 参数，与使用默认方式调用时 fail 回调中的 err 参数一致
							uni.hideLoading();
							// console.log(err);
							uni.showToast({
								title: '提交错误',
								icon: 'error'
							});
							// this.resetFormData();
						});
				})
				.catch(err => {
					uni.showToast({
						icon: 'error',
						title: '上传数据格式错误'
					});
					console.log('表单错误信息：', err);
				});
		}
		//提交按钮触发事件 end
	}
};
</script>

<style></style>
