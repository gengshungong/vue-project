<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>
            <el-form label-width="0px" class="login_form" :model="loginForm" :rules="loginFormRules" ref="loginFormRef">
                <el-form-item prop="username">
                    <el-input prefix-icon="el-icon-user" v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input prefix-icon="el-icon-lock" v-model="loginForm.password" type="password"></el-input>
                </el-form-item> 
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登陆</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item> 
            </el-form>
        </div>
    </div>
</template>
<script>
export default {
    // 数据
    data(){
        return{
            loginForm:{
                username:'admin',
                password:'123456'
            },
            loginFormRules:{
                username:[
                    { required: true, message: '请输入账号', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true,message: "请输入密码", trigger: "blur"},
                    {min:6,max:15,message:"长度在6到15个字符之间",trigger:"blur"}
                ]
            }
        }
    },
    methods:{
        resetLoginForm(){
            this.$refs.loginFormRef.resetFields();
        },
        login(){
            this.$refs.loginFormRef.validate(async valid =>{
                if(!valid) return;
                const {data:res} =await this.$http.post("login",this.loginForm);
                if(res.meta.status !==200) return this.$message.error('登陆失败');
                this.$message.success('登陆成功');
                //这里要完成登陆成功之后页面的跳转，有以下步骤：
                //1.登陆成功之后，会返回一个token，将这个token保存到sessionStorage中，
                //1.1项目中存在其他api接口，必须在登陆完成之后，才可以查看
                //1.2token只需要它在页面打开时生效，而localStorage是永久保存，所以要保存在sessionStorage
                window.sessionStorage.setItem("token",res.data.token);
                //2.通过路由导航跳转到后台页面，路由地址/home
                this.$router.push("/home")
            });
        }
    }
}
</script>
<style lang="less" scoped>
    .login_container{
        background-color: #2b4b6b;
        height: 100%;
        position: relative;
    }
    .login_box{
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 3px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        
        .avatar_box{
            height: 130px;
            width: 130px;
            border-radius: 50%;
            padding: 10px;
            border: 1px solid #eee;
            box-shadow: 0 0 10px #ddd;
            position: absolute;
            left: 50%;
            transform: translate(-50%,-50%);
            background-color: #fff;
            img{
                width: 100%;
                height: 100%;
                border-radius: 50%;
                background-color: #eee;
            }
        }
    }
    .login_form{
        position: absolute;
        bottom: 0;
        width: 100%;
        padding: 0 20px;
        box-sizing: border-box;
    }
    .btns{
        display: flex;
        justify-content: flex-end;
        
    }
</style>