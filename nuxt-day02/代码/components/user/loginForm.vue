<template>
    <!-- model: 表示声明当前表单的数据对象（这个表单最终的数据存放到data的form对象里面） -->
    <!-- ref: 获取dom元素 -->
    <!-- rules: 表单的验证规则 -->
    <el-form 
        :model="form" 
        ref="form"
        :rules="rules" 
        class="form">
        
        <el-form-item class="form-item" prop="username">
            <!-- 用户名输入框 -->
            <el-input 
            placeholder="用户名/手机" 
            v-model="form.username">
            </el-input>
        </el-form-item>

        <el-form-item class="form-item" prop="password">
            <!-- 密码输入框 -->
            <el-input 
            placeholder="密码" 
            type="password"
            v-model="form.password">
            </el-input>
        </el-form-item>

        <p class="form-text">
            <nuxt-link to="#">忘记密码</nuxt-link>
        </p>

        <el-button 
        class="submit"
        type="primary"
        @click="handleLoginSubmit">
            登录
        </el-button>
    </el-form>

</template>

<script>
export default {
    data(){
        return {
            // 表单数据
            form: {
                username: "", // 用户名
                password: "" // 密码
            },
            // 表单规则，里面的输入必须和form统一，上面的属性叫什么下面也得统一
            rules: {
                // 手机号码的验证规则，
                username: [
                    { required: true, message: "请输入手机号码", trigger: "blur" }
                ],
                password: [
                    { required: true, message: "请输入密码", trigger: "blur" }
                ]
            },
        }
    },
    methods: {
        // 提交登录
        handleLoginSubmit(){
            // 判断element的表单验证是否通过, element的表单都有一个validate这个方法
            this.$refs.form.validate((valid) => {
                // valid如果值是true代表验证通过
                if(valid){
                    // 请求登录接口
                    this.$axios({
                        url: "/accounts/login",
                        method: "POST",
                        data: this.form
                    }).then(res => {
                        const {data} = res;

                        // 这个写法是错的
                        // this.$store.state.user.userInfo = data;

                        // 通过this.$store.commit调用mutations的方法
                        this.$store.commit('user/setUserInfo', data);
                    })
                }
            })
        }
    }
}
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>
