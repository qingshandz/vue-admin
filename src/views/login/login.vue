<template>
   <div id="login">
      <div class="login-wrap">
         <ul class="menu-tab">
             <li  v-for="item in menuTab" :key="item.id" :class="{'current': item.current}"  @click="tiggerMenu(item)">{{item.text}}</li>
         </ul>
         <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm"  class="login-form" size="medium">
            <el-form-item  prop="pass" class = "item-form">
               <label>邮箱</label>
               <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item  prop="checkPass" class = "item-form">
               <label>密码</label>
               <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item  prop="age" class = "item-form">
               <label>验证码</label>
               <el-input v-model.number="ruleForm.age"></el-input>
            </el-form-item>
            <el-form-item>
               <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
            </el-form-item>
         </el-form>
      </div>
   </div>
</template>
<script>
export  default {
   name: "login",
   data(){
      var checkAge = (rule, value, callback) => {
         if (!value) {
            return callback(new Error('年龄不能为空'));
         }
         setTimeout(() => {
            if (!Number.isInteger(value)) {
               callback(new Error('请输入数字值'));
            } else {
               if (value < 18) {
                  callback(new Error('必须年满18岁'));
               } else {
                  callback();
               }
            }
         }, 1000);
      };
      var validatePass = (rule, value, callback) => {
         if (value === '') {
            callback(new Error('请输入密码'));
         } else {
            if (this.ruleForm.checkPass !== '') {
               this.$refs.ruleForm.validateField('checkPass');
            }
            callback();
         }
      };
      var validatePass2 = (rule, value, callback) => {
         if (value === '') {
            callback(new Error('请再次输入密码'));
         } else if (value !== this.ruleForm.pass) {
            callback(new Error('两次输入密码不一致!'));
         } else {
            callback();
         }
      }
      return {
         menuTab:[
            {text: "登录", current:true},
            {text: "注册", current:false}
         ],
         ruleForm: {
            pass: '',
            checkPass: '',
            age: ''
         },
         rules: {
            pass: [
               { validator: validatePass, trigger: 'blur' }
            ],
            checkPass: [
               { validator: validatePass2, trigger: 'blur' }
            ],
            age: [
               { validator: checkAge, trigger: 'blur' }
            ]
         }
      };
   },
   created() {
   },
   mounted() {
   },
   methods: {
      tiggerMenu(data){
         this.menuTab.forEach(elem=>{
            elem.current = false;
         })
         data.current = true;
      },
      submitForm(formName) {
         this.$refs[formName].validate((valid) => {
            if (valid) {
               alert('submit!');
            } else {
               console.log('error submit!!');
               return false;
            }
         });
      }
   }
}
</script>
<style lang="scss" scoped>
 #login {
    height: 100vh;
    background: #344a5f
}
 .login-wrap {
    width: 500px;
    margin: auto;

 }
 .menu-tab {
    text-align: center;
    li {
       display: inline-block;
       width: 120px;
       line-height: 60px;
       font-size: 14px;
       color: #ffffff;
       border-radius: 2px;
       cursor: pointer;
    }
 }
 .current {
    /*background-color: rgba(0,0,0,0.1);*/
    background-color: #4682B4;
 }
.login-form {
   margin-top: 20px;
   label {
      font-size: 14px;
      color: white;
      display: block;
      margin-bottom: 15px;
   }
   item-form {
      margin-bottom: 15px;
   }
}
</style>