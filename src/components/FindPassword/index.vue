<template>
    <div class="findPassword_body">
        <div class="findPassword_email">
            邮箱: <input v-model="email" class="findPassword_text" type="text"> <button @touchstart="handleToverify">发送验证码</button>
        </div>
        <div>
            新密码: <input v-model="password" class="findPassword_text" type="password">
        </div>
        <div>
            验证码: <input v-model="verify" class="findPassword_text" type="text">
        </div>
        <div class="findPassword_btn">
            <button @touchstart="handleToPassword">确认修改</button>
        </div>
        <div class="findPassword_link">
            <router-link to="/mine/login">立即登录</router-link>
            <router-link to="/mine/register">立即注册</router-link>
        </div>
    </div>
</template>

<script>
import { messageBox } from '@/components/JS'
export default {
    name: 'findPassword',
    data(){
        return {
            email:'',
            password:'',
            verify:''
        }
    },
    methods: {
        handleToverify(){
            this.axios.get('/api2/users/verify?email='+ this.email).then((res)=>{
                var status = res.data.status;
                if( status === 0 ){
                    messageBox({
                        title: '验证码',
                        content: '验证码发送成功',
                        ok: '确定'
                    });
                }else{
                    messageBox({
                        title: '验证码',
                        content: '验证码发送失败',
                        ok: '确定'
                    });
                }
            });
        },
        handleToPassword(){
            this.axios.post('/api2/users/findPassword',{
                email: this.email,
                password: this.password,
                verify: this.verify
            }).then((res) => {
                var status = res.data.status;
                var This = this;
                if(status === 0){
                    messageBox({
                        title: '修改',
                        content: '修改密码成功',
                        ok: '确定',
                        handleOk(){
                            This.$router.push('/mine/login')
                        }
                    });
                }else{
                    messageBox({
                        title: '修改',
                        content: '修改密码失败',
                        ok: '确定',
                    });
                }
            });
        }
    }
}
</script>

<style scoped>
#content .findPassword_body{  width:100%;}
.findPassword_body .findPassword_text{ width:100%; height: 40px; border:none; border-bottom: 1px #ccc solid; margin-bottom: 5px; outline: none; text-indent: 10px;}
.findPassword_body .findPassword_btn{ height:50px; margin:10px;}
.findPassword_body .findPassword_email{position: relative;}
.findPassword_body .findPassword_email button {position: absolute;right: 10px;height: 30px;border-radius: 3px; border:none;padding: 5px}
.findPassword_body .findPassword_btn button{ width:100%; height:100%; background:#e54847; border-radius: 3px; border:none; display: block; color:white; font-size: 20px;}
.findPassword_body .findPassword_link{ display: flex; justify-content:space-between;}
.findPassword_body .findPassword_link a{ text-decoration: none; margin:0 5px; font-size: 12px; color:#e54847;}
</style>