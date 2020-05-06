<template>
    <div>
        <!-- 验证规则rules,对象loginForm,太丑了写个style .loginContainer-->
        <el-form :rules="rules" ref="loginForm" :model="loginForm" class="loginContainer">
            <h3 class="loginTitle">系统登陆</h3>
            <!-- prop数据传递-->
            <el-form-item prop="username">
                <!-- auto-complete：自动补全 placeholder:占位符-->
                <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <!-- auto-complete：自动补全 placeholder:占位符-->
                <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="请输入密码"></el-input>
            </el-form-item>
            <!-- 记住账号密码-->
            <el-checkbox v-model="checked">记住账号密码</el-checkbox>
            <el-button type="primary" style="width: 100%" @click="submitLogin">登录</el-button>
        </el-form>
    </div>
</template>

<script>

    export default {
        name: "Login",
        data(){
            return{
                loginForm:{
                    username:'admin',
                    password:'123'
                },
                checked:true,
                rules:{
                    username:[{required:true,message:'请输入用户名',trigger:'blur'}],
                    password:[{required:true,message:'请输入密码',trigger:'blur'}],
                }
            }
        },
        methods:{
            submitLogin(){
                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        this.postKeyValueRequest('/doLogin',this.loginForm).then(resp=>{
                            if(resp){
                                window.sessionStorage.setItem("user",JSON.stringify(resp.obj));
                                this.$router.replace('/home');
                            }
                        })
                    } else {
                        this.$message.error('请输入所有字段');
                        return false;
                    }
                });
            },
        }
    }
</script>

<style>
    .loginTitle{
     margin: 20px auto 40px auto;
     text-align: center;
    }
    .loginContainer{
        border-radius: 15px;
        background-clip: padding-box;
        margin: 180px auto;
        width: 350px;
        padding:25px;
        background: #ffffff;
        border: 1px solid burlywood;
        box-shadow: 0 0 25px #cac6c6;
    }
</style>