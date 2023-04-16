<template>
    <el-form ref="ruleFormRef" :model="state.ruleForm" :rules="rules" :size="formSize">
        <el-form-item prop="phone">
            <el-input v-model="state.ruleForm.phone" placeholder="手机号" />
        </el-form-item>
        <el-form-item prop="password">
            <el-input v-model="state.ruleForm.password" type="password" show-password placeholder="密码" />
        </el-form-item>
        <el-form-item prop="isRemember">
            <el-checkbox v-model="state.ruleForm.isRemember" label="记住密码" />
            <el-button type="text">忘记密码</el-button>
            <el-button @click="loginOut" type="text">退出登录</el-button>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submitForm(ruleFormRef)">
                登录
            </el-button>
        </el-form-item>
    </el-form>
</template>

<script lang="ts" setup>
import { reactive, ref, onMounted } from 'vue'
import type { FormInstance, FormRules, ElMessage } from 'element-plus'

const formSize = ref('default')
const ruleFormRef = ref<FormInstance>()
let state = reactive({
    ruleForm: {
        phone: '',
        password: '',
        isRemember: false
    }

})

onMounted(() => {
    // const account = getCookie("account")
    // if (account) {
    //     state.ruleForm = JSON.parse(account)
    // }
    // 获取登录凭证
    if (navigator.credentials) {
        navigator.credentials.get({
            password: true
        }).then(cred => {
            if (cred) {
                switch (cred.type) {
                    case 'password':
                        // 此处为自动填充表单的代码
                        // 开发者可以根据实际需要对账户信息进行其他处理
                        state.ruleForm.phone = cred.id;
                        state.ruleForm.password = cred.password;
                        break;
                    default: break;
                }
            }
        });
    }

})
const checkPhone = (rule: any, value: any, callback: any) => {
    if (!value) {
        return callback(new Error('手机号不能为空'));
    } else {
        //验证手机号
        const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
        if (!reg.test(value))
            return callback(new Error('手机号码无效'));
        else
            callback()
    }
};
const rules = reactive<FormRules>({
    phone: [{ required: true, validator: checkPhone, trigger: "blur" }],
    password: [
        { required: true, message: '密码必填', trigger: 'blur' },
        { min: 6, message: '密码长度至少是6位', trigger: 'blur' },
    ],
})

const submitForm = async (formEl: FormInstance | undefined) => {
    if (!formEl) return
    await formEl.validate((valid) => {
        if (valid) {
            if (state.ruleForm.isRemember) {
                rememberPwd()
                // setCookie("account", 1)
            } else {
                ElMessage.success('登录成功，没有记住密码')
                // setCookie("account", 0)
            }
        } else {
            console.log("valid2")
        }
    })
}
function rememberPwd() {
    if (navigator.credentials) {        // 生成密码凭据
        const credential = new PasswordCredential({
            password: state.ruleForm.password,
            id: state.ruleForm.phone,
        });
        navigator.credentials.store(credential).then(() => {
            ElMessage.success('登录成功，并记住密码')
        });
    }
}
function loginOut() {
    // 处理登出流程
    if (navigator.credentials.requireUserMediation) {
        navigator.credentials.requireUserMediation();
    }
    // Chrome 60 + 后，requireUserMediation 被重命名为 preventSilentAccess
    else if (navigator.credentials.preventSilentAccess) {
        navigator.credentials.preventSilentAccess();
    }
}
// 记住密码
const setCookie = function (cname: string, exdays: number) {
    let d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    document.cookie = cname + '=' + JSON.stringify(state.ruleForm) + ";expires=" + d.toUTCString()
    ElMessage.success('提交成功')
}
const getCookie = function (cname: string) {
    let name = cname + "=";
    let ca = document.cookie.split(';');
    for (let i = 0, len = ca.length; i < len; i++) {
        let c = ca[i].trim();
        if (c.indexOf(name) == 0) return c.substring(name.length, c.length);
    }
    return "";
}


</script>
<style scoped lang="scss">
::v-deep .el-form-item__content {
    justify-content: space-between;
}
</style>
