

<template>
  <div class="register">
    <van-nav-bar
      title="注册"
      left-text="登录"
      right-text="关闭"
      @click-left="goPage('Login')"
    />
    <div class="logo">
      <img class="auto-img" src="../assets/images/logo.png" alt="">
    </div>

    <van-form @submit="register">
      <van-field
        v-model="userInfo.phone"
       
        type="text"
        label="手机号"
        placeholder="手机号"
        clearable
         right-icon="close"
        left-icon="phone-o"
      >
        
      </van-field>
      <van-field
        v-model="userInfo.nickName"
        
        type="text"
        label="昵称"
        placeholder="昵称"
        left-icon="contact"
        right-icon="smile-o"
      />
      <van-field
        v-model="userInfo.password"
       
        :type="isEye ? 'text': 'password'"
        label="密码"
        placeholder="密码"
        left-icon="lock"
        :right-icon="isEye ? 'eye-o': 'closed-eye'"
         @click-right-icon="viewPassword"
      />
      <div class="register-btn" >
        <van-button round block type="info" native-type="submit" class="zhuce">注册</van-button>
      </div>
    </van-form>
  </div>
</template>
 
<script>

import  '../assets/css/register.less'
//导入验证表单模块
import validForm from '../assets/js/validForm'
  export default {
    data(){
      return{
        //用户注册信息
        userInfo:{
          phone:'',
          nickName:'',
          password:''
        },

        //false:闭合，true：睁开
        isEye:false
      }
    },

    methods:{

      //跳转页面
      goPage(name){
        this.$router.push({name});
      },

      //切换密码格式
      viewPassword(){
        this.isEye=!this.isEye;
      },

     //注册
      register() {

        //进行验证表单
        if (validForm.validUserForm(this.userInfo)) {

          //复制对象
          let data = Object.assign({}, this.userInfo);

          //请求密钥
          data.appkey = this.appkey;

          //开启加载提示
          this.$toast.loading({
            message: '加载中...',
            forbidClick: true,

            //延迟自动关闭加载提示，如果该值为0，则不会自动关闭
            duration:0
          });

          //发起注册请求
          this.axios({
            method: 'POST',
            url: '/register',

            //post传递参数需要写在data
            data
          }).then(result => {
            //关闭加载提示
            this.$toast.clear();
            

            if(result.data.code == 100){
              //跳转到登录界面
              this.$router.push({name:'Login'})
            }else{
              this.$toast(result.data.msg);
            }
          }).catch(err => {
            //关闭加载提示
            this.$toast.clear();
            
          })
        }

      }
    }
  }
</script>


