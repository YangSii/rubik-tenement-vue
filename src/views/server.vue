<template>
    <div class="server">
        <h3>服务派单</h3>
        <el-form :model="detail" ref="detail" label-width="130px" class="demo-detail" :rules="rules" :disabled="edit">
        <div class="tianjia">
            <div class="input">
                <el-row>
                    <el-col :span="12">
                    <el-form-item label="姓名:" prop="name">
                        <el-input v-model="detail.name" v-on:blur="transformName" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="关联房屋:" prop="house">
                        <el-cascader expand-trigger="hover" :options="options" v-model="detail.house" @change="handleChange" style="width:100%"></el-cascader>
                        
                    </el-form-item>
                    <el-form-item label="服务类别:">
                        
                        <el-select v-model="detail.service_classes" placeholder="服务类别" style="width:100%">
                            <el-option
								v-for="item in service"
								:key="item.value"
								:label="item.value"
								:value="item.value">
							</el-option>
                        </el-select>
                    </el-form-item>
                     <el-row>
                         <el-col :span="12">
                            <el-form-item label="受理时间:" prop="receiverTime">
                                <el-date-picker
                                v-model="detail.receiverTime"
                                type="datetime"
                                format="yyyy/MM/dd HH:mm:ss" value-format="yyyy/MM/dd HH:mm:ss"
                                placeholder="选择日期时间"
                                style="width:100%">
                                </el-date-picker>
                            </el-form-item>
                         </el-col>
                         <el-col :span="12">
                            <el-form-item label="开工时间:">
                                <el-date-picker
                                    v-model="detail.startTime"
                                    type="datetime"
                                    format="yyyy/MM/dd HH:mm:ss" value-format="yyyy/MM/dd HH:mm:ss"
                                    placeholder="选择日期时间"
                                    style="width:100%">
                                </el-date-picker>
                            </el-form-item>
                         </el-col>
                     </el-row>
                    
                    <el-form-item label="派工至:">
                        <el-select v-model="detail.handler" placeholder="服务类别" style="width:100%" @change="findUsers">
                            <el-option
								v-for="items in serviceTo"
								:key="items.id"
								:label="items.label"
								:value="items.id">
							</el-option>
                        </el-select>
                        <!-- <el-input v-model="detail.handler" clearable></el-input> -->
                    </el-form-item>
                    </el-col>
                    <el-col :span="12">
                    <el-form-item label="手机号：" prop="phone">
                        <el-input v-model="detail.phone" v-on:blur="transform" clearable></el-input>
                    </el-form-item>
                    <el-form-item label="要求处理事项：">
                        <el-input placeholder="请输入内容" v-model="detail.problem">
                    </el-input>
        </el-form-item>
                    <!-- <el-form-item label="类型：">
                        <el-input v-model="detail.leaseType" clearable></el-input>
                    </el-form-item> -->
                    <el-form-item label="报修方式:">
                        <el-select v-model="detail.way" placeholder="报修方式" style="width:100%">
                            <el-option
								v-for="item in ways"
								:key="item.value"
								:label="item.value"
								:value="item.value">
							</el-option>
                        </el-select>
                    </el-form-item>
                    <el-row>
                        <el-col :span="12">
                        <el-form-item label="派工时间:">
                            <el-date-picker
							v-model="detail.dispatchingTime"
							type="datetime"
                            format="yyyy/MM/dd HH:mm:ss" value-format="yyyy/MM/dd HH:mm:ss"
							placeholder="选择日期时间"
                            style="width:100%">
                        </el-date-picker>
                    </el-form-item>
                        </el-col>
                        <el-col :span="12">
                    <el-form-item label="完工时间:">
                        <el-date-picker
							v-model="detail.endTime"
							type="datetime"
                            format="yyyy/MM/dd HH:mm:ss" value-format="yyyy/MM/dd HH:mm:ss"
							placeholder="选择日期时间"
                            style="width:100%">
                        </el-date-picker>
                    </el-form-item>
                        </el-col>
                    </el-row>
                    <el-form-item label="专业分类:">
                        <el-select v-model="detail.professional_lis" placeholder="服务类别" style="width:100%">
                            <el-option
								v-for="find in handlerTo"
								:key="find.id"
								:label="find.name"
								:value="find.id">
							</el-option>
                        </el-select>
                        <!-- <el-input v-model="detail.professional_list" clearable></el-input> -->
                    </el-form-item>
                    </el-col>
                </el-row>
                    
            </div>
        </div>
        <div style="width:90%">
            
        <el-form-item label="备注：">
            <el-input type="textarea" :rows="8" placeholder="请输入内容" v-model="detail.remarks">
        </el-input>
        </el-form-item>
        </div>
        </el-form>
        <div class="nn">
            <button class="nextStep" @click="addOne">保存</button><button class="cancel" @click="goBack">返回</button>
            </div>
    </div>
</template>

<script>

import url from '../assets/Req.js'

export default {
    name:'server',
    data(){
        return{
            detail: {
                name: '',
                area: '',
                house: [],
                server:'',
                startTime: '',
                sendTime: '',
                to:'',
                phone: '',
                mold: '',
                type: '',
                endTime:'',
                time:'',
                card: '',
                person:'',
                require: '',
                textarea:'',
                handler: ''
            },
            service:[],
            serviceTo: [],
            handlerTo: [],
            ways:[],
            options: [],
        rules: {
          name: [
            { required: true, message: '请输入姓名', trigger: 'blur' },
          ],
          house:[
              { required: true, message: '请选择关联房屋', trigger: 'change'}
          ],
          phone: [
            { required: true, message: '请输入手机号', trigger: 'blur' },
          ],
          receiverTime: [
              { required: true, message: '请选择受理时间', trigger: 'blur' }
          ]
          },
          edit:true  
        }
    },
    mounted(){
        this.$ajax.get(url + 'company/findAll').then(res => {
            this.serviceTo = res.data.data
        })
         this.id = this.$route.query.id
         this.$ajax.get(url + 'serviceAccept/findByDictType/1').then(res => {
                this.service=res.data;
         })
         this.$ajax.get(url + 'serviceAccept/findByDictType/2').then(res => {
                this.ways=res.data;
         })
        if(this.$route.query.msg == 8){
            this.$ajax.get(url +'serviceAccept/findIdVO/'+this.id).then(res => {
                if(res.status === 200){
                    this.detail = res.data;
                    this.detail.house = [res.data.precinct, res.data.buildings, res.data.room];
                    this.editChange(res.data.name,res.data.phone)
                    this.detail.handler = Number(res.data.handler)
                    this.findUsers()
                }else if(res.status===403){
                    this.$alert('您的权限不足', '权限不足', {
                        confirmButtonText: '确定',
                        callback: action => {
                            this.goBack()
                        }
                    });
                }
            })
            this.edit = true
        }else if(this.$route.query.msg == 7){
            this.$ajax.get(url +'serviceAccept/findIdVO/'+this.id).then(res => {
                this.detail = res.data;
                this.detail.house = [res.data.precinct, res.data.buildings, res.data.room];
                this.editChange(res.data.name,res.data.phone)
                this.detail.handler = Number(res.data.handler)
                this.findUsers()
                this.detail.professional_list = res.data.professional_list
            })
            this.edit = false
        }else(
            this.edit = false,
            this.datail = '',
            this.$ajax.get(url + 'room/flndByClientId/aaa'+'').then(res => {
                this.options=res.data;
            })
        )
    },
    methods: {
        findUsers(){
            this.$ajax.get(url+'company/findUsers',{
                params:{
                    "id": this.detail.handler
                }
            }).then(res => {
                this.handlerTo = res.data.data
            })
        },
        blur (e) {
            var reg = /^\+?[1-9][0-9]*$/
            if (!reg.test(e.target.value)) {
                e.target.style.borderColor = 'red'
                this.$message({
                message: '请输入数字',
                type: 'error'
                })
            }else if(e.target.value.length!==11){
                e.target.style.borderColor = 'red'
                this.$message({
                message: '请输入11位数字',
                type: 'error'
                })
            } else {
                e.target.style.borderColor = '#67c23a'
            }
        },
    
    editChange(a,b){
            this.$ajax.get(url + 'owner/findByNameAndPhone/'+a+'/'+b).then(res => {
                var aa = "";
                    if(!res.data){
                        aa = "aaa";
                    }else{
                        aa = res.data.id;
                    }
                    this.$ajax.get(url + 'room/flndByClientId/'+aa).then(res => {
                        this.options=res.data;
                    })
                })
        },
    //失去焦点事件，当移开姓名时判断
      transformName:function(){
          if(!this.detail.name){
              this.$message({
                message: '请先输入业主姓名',
                 type: 'error'
               })
            
          }else{
              if(!this.detail.phone){
                  this.$ajax.get(url + 'owner/findByName/'+this.detail.name).then(res => {
                       if(res.data.length==0){
                           this.$message({
                                message: '没有找到该业主的任何信息',
                                type: 'error'
                            })
                          
                         this.detail.name = null;
                       }
                   })
                 }else{
                      this.$ajax.get(url + 'owner/findByNameAndPhone/'+this.detail.name+'/'+this.detail.phone).then(res => {
                        if(!res.data){
                            this.detail.phone = null;
                        }
                    })
                 }
              }
      },
    //失去焦点事件，当移开电话时判断
      transform:function(){
          if(!this.detail.name){
               this.$message({
                      message: '请先输入业主姓名',
                    type: 'error'
                }) 

          }else if(!this.detail.phone){
                         this.$message({
                                message: '请输入电话号码',
                                type: 'error'
                            }) 

          }else{
           this.$ajax.get(url + 'owner/findByNameAndPhone/'+this.detail.name+'/'+this.detail.phone).then(res => {
                var aa = "";
                if(!res.data){
                     this.$message({
                                message: '业主绑定的电话号码有误，请重新输入！',
                                type: 'error'
                            })
                    this.detail.phone = null;
                    aa = "aaa";
                }else{
                    aa = res.data.id;
                }
                 this.$ajax.get(url + 'room/flndByClientId/'+aa).then(res => {
                     this.options=res.data;
                 })
            })
          }
      },

    //修改或新增服务派工
	   addOne(){
            var serviceAcceptVO={};
            serviceAcceptVO.name=this.detail.name;           //租户姓名
            serviceAcceptVO.service_classes=this.detail.service_classes;   //服务类别
            var arr=this.detail.house;
            serviceAcceptVO.roomNumber=arr[arr.length-1];//关联房屋
            serviceAcceptVO.startTime=this.detail.startTime;    //开始时间
            serviceAcceptVO.endTime=this.detail.endTime;    //结束时间
            serviceAcceptVO.dispatchingTime=this.detail.dispatchingTime;    //派工时间
            serviceAcceptVO.handler=this.detail.handler;    //派工至
            serviceAcceptVO.phone=this.detail.phone;    //手机号
            serviceAcceptVO.leaseType=this.detail.leaseType;   //类型
            serviceAcceptVO.way=this.detail.way;   //报修方式
            serviceAcceptVO.receiverTime=this.detail.receiverTime;    //工时
            serviceAcceptVO.professional_list=this.detail.professional_list;    //专业分类
            serviceAcceptVO.problem=this.detail.problem;    //要求处理事项
            serviceAcceptVO.remarks=this.detail.remarks;    //备注
            if(this.$route.query.msg == 7){
                 serviceAcceptVO.id = this.id;
                this.$ajax.put(url+"serviceAccept/update",serviceAcceptVO).then((res) => {
                    this.form = res.data
                    if(res.data.status === 200){
                         this.$message({
                                message: '修改数据成功',
                                type: 'success'
                            }),
                            this.goBack()
                        }else if(res.data.status===403){
                            this.$message({
                                message:'权限不足',
                                type: 'error'
                            })
                        }else{
                            this.$message({
                                message: res.data.msg,
                                type: 'error'
                        }) 
                     }
                })
            }else if (this.$route.query.msg == 8){
                this.goBack()
            }else{
                this.$ajax.post(url+"serviceAccept/insert",serviceAcceptVO).then((res) => {
                    this.form = res.data
                     if(res.data.status === 200){
                         this.$message({
                                 message: '增加数据成功',
                                type: 'success'
                            }),
                            this.goBack()
                        }else if(res.data.status===403){
                            this.$message({
                                message:'权限不足',
                                type: 'error'
                            })
                        }else{
                            this.$message({
                                message: res.data.msg,
                                type: 'error'
                        }) 
                     }
                   
                })
            }
            
        },
      goBack(){
          window.history.back()
      },
      handleChange(value) {
          this.$ajax.get(url + 'owner/findOwnerByRoomId/'+value[2]).then(res => {
              if(res.data.status === 200){
                  this.detail.name = res.data.data.name
                  this.detail.phone = res.data.data.phone
              }else{
                  this.$message({
                      message:res.data.msg,
                      type: 'error'
                  })
                  this.detail.name = '',
                  this.detail.phone = ''
              }
          })
      }
    }
}
</script>

<style scoped>
.server {
    position: absolute;
    background: white;
    z-index: 2000;
    width: 100%;
    height: 100%;
}
.tianjia{
    vertical-align: top;
    width: 90%;
}


/* .input {
    width: 40%;
    margin: 0 0 0 24%;
    min-width: 500px;
} */
.nextStep {
  border-radius: 5px;
  background-color: rgb(50, 168, 238);
  width: 65px;
  height: 33px;
  border: none;
  color: white;
}

.cancel {
  border-width: 1px;
  border-color: rgb(217, 217, 217);
  border-style: solid;
  border-radius: 5px;
  background-color: rgb(255, 255, 255);
  width: 63px;
  height: 33px;
  font-size: 14px;
  font-family: "Microsoft YaHei";
  color: rgb(138, 138, 138);
}
.nn {
    width: 20%;
    margin: 4% 0 0 40%;
    display: flex;
    justify-content: space-between;
}

form {
    margin-top: 35px;
}
.zhuangxiu {
    width: 49%;
    display: inline-block;
    vertical-align: top;
}
/* .el-radio{
    display: block;
    margin-left: 0;
    margin-top: 13px;
    margin-bottom: 52px;
}
.el-form-item__label {

} */
</style>
