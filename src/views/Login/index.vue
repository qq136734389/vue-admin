<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li  v-for="item in menuTab" :key="item.id" :class="{'current': item.current}" @click="toggleMenu(item)">{{ item.txt }}</li>
            </ul>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm"  class="login-form" size="medium">
                
                <el-form-item  prop="userName" class="item-form">
                    <label>邮箱</label>
                    <el-input type="text" v-model="ruleForm.userName" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item  prop="password" class="item-form">
                    <label>密码</label>
                    <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength='6' maxlength='20'></el-input>
                </el-form-item>

                <el-form-item  prop="passwords" class="item-form" v-show="model === 'register'">
                    <label>重置密码</label>
                    <el-input type="password" v-model="ruleForm.passwords" autocomplete="off" minlength='6' maxlength='20'></el-input>
                </el-form-item>

                <el-form-item  prop="code" class="item-form">                   
                    <label>验证码</label>
                    <el-row :gutter="10">
                        <el-col :span="15">
                            <el-input v-model.number="ruleForm.code"></el-input>
                        </el-col>
                        <el-col :span="9">
                            <el-button type="success" class="block">获取验证码</el-button>   
                        </el-col>
                    </el-row>
                </el-form-item>

                <el-form-item>
                    <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn-10 block">提交</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
import {stripscript,validatePassword1,validateEmail1,validateCode} from '@/utils/validate';
export default {
    name: 'login',
    data(){
        
        //验证用户名
        var validateuserName = (rule, value, callback) => {
            let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
            if (value === '') {
                callback(new Error('请输入用户名'));
            } else if (validateEmail1(value)){                
                callback(new Error('用户名格式有误'));
            } else {
                callback()
            }
        };

        //验证密码
        var validatePassword = (rule, value, callback) => {
            this.ruleForm.password = stripscript(value)
            value = this.ruleForm.password
            
            if (value === '') {
                callback(new Error('请输入密码'));
            } else if (validatePassword1(value) ) {
                callback(new Error('密码为6至20位数字+字母'));
            } else {
                callback();
            }
        };

        //验证重复密码
        var validatePasswords = (rule, value, callback) => {
            this.ruleForm.passwords = stripscript(value)
            value = this.ruleForm.passwords
            
            if (value === '') {
                callback(new Error('请再次输入密码'));
            } else if ( value != this.ruleForm.password ) {
                callback(new Error('两次输入的密码不一致'));
            } else {
                callback();
            }
        };

        //验证验证码
        var checkAge = (rule, value, callback) => {
            this.ruleForm.password = stripscript(value)
            value = this.ruleForm.password
            let reg = /^[a-z0-9]{6}$/
            if (value === '') {
                return callback(new Error('请输入验证码'));
            } else if (validateCode(value)) {
                return callback(new Error('验证码格式有误'));
            } else {
                callback();
            }
        };
        return {
            menuTab: [
                { txt: '登录', current: true, type: 'login'},
                { txt: '注册', current: false, type: 'register'}
            ],
            model: 'login',
            ruleForm: {
                userName: '',
                password: '',
                code: '',
                passwords: ''
            },
            rules: {
                userName: [
                    { validator: validateuserName, trigger: 'blur' }
                ],
                password: [
                    { validator: validatePassword, trigger: 'blur' }
                ],
                code: [
                    { validator: checkAge, trigger: 'blur' }
                ],
                passwords: [
                    { validator: validatePasswords, trigger: 'blur' }
                ]
            }
        }
    },
    methods: {
        toggleMenu(data) {
            this.menuTab.forEach(ele => {
                ele.current = false
            });
            
            data.current = true;
            this.model = data.type;
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
    #login{
        background-color: #344a5f;
        height: 100vh;
    }
    .login-wrap{
        width: 330px;
        margin: auto;
    }
    .menu-tab {
        text-align: center;
        li {
            display: inline-block;
            width: 88px;
            line-height: 36px;
            font-size: 14px;
            color: #fff;
            border-radius: 2px;
            cursor: pointer;
        }
    }
    .current{
        background-color: rgba(0, 0, 0, .1);
    }

    .login-form {
        margin-top: 20px;
        label{
            display: block;
            margin-bottom: 3px;
            font-size: 14px;
            color: #fff;
        }
        .item-form{
            margin-bottom: 13px;
        }
        .block {
            display: block;
            width: 100%;
        }
        .login-btn-10{
            margin-top: 10px;
        }
    }
</style>