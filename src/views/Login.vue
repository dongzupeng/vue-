

<template>
  <div class="login">
    <van-nav-bar
      title="登录"
      left-text="注册"
      right-text="关闭"
      @click-left="goPage('Register')"
    />
    <div class="logo">
      <img class="auto-img" src="../assets/images/logo.png" alt="">
    </div>

    <div class="title">Drink</div>

    <van-form @submit="login">
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
        v-model="userInfo.password"
       
        :type="isEye ? 'text': 'password'"
        label="密码"
        placeholder="密码"
        left-icon="lock"
        :right-icon="isEye ? 'eye-o': 'closed-eye'"
         @click-right-icon="viewPassword"
      />
      <div class="register-btn" >
        <van-button round block type="info" native-type="submit" class="zhuce">登录</van-button>

        <div class="forgot" @click="goPage('Forget')">
            <span class="fr">忘记密码？</span>
        </div>
      </div>
    </van-form>
  </div>
</template>
 
<script>

import '../assets/css/register.less'
//导入验证表单模块
import validForm from '../assets/js/validForm'
  export default {
    data(){
      return{
        //用户注册信息
        userInfo:{
          phone:'',
          password:''
        },

        //false:闭合，true：睁开
        isEye:false
      }
    },

    methods:{
      //跳转页面
      goPage(name){
          this.$router.push({name})
      },
      //切换密码格式
      viewPassword(){
        this.isEye=!this.isEye;
      },

     //注册
      login() {

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
            url: '/login',

            //post传递参数需要写在data
            data
          }).then(result => {
            //关闭加载提示
            this.$toast.clear();
            
            if(result.data.code==200){
                //跳转到菜单Menu
                 this.$router.push({name: 'Menu'});
                

                //存储token，用于后面登录验证
                localStorage.setItem('&tk',result.data.token);
            }else {
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


