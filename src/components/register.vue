<template>
    <div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#username"></use>
                    </svg>
                </div>
                <userInput v-model.trim="username" class="inputbox transparent inline-block vertical-align"></userInput>
            </div>
            <div v-if="$v.username.required && !$v.username.isUnique" class="check tip-color min-font">用户名已注册
            </div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#email"></use>
                    </svg>
                </div>
                <eInput v-model.trim="emailInput" class="inputbox transparent inline-block vertical-align"></eInput>
            </div>
            <div v-if="$v.emailInput.email && $v.emailInput.required && !$v.emailInput.isUnique" class="check tip-color min-font">邮箱已注册
            </div>
            <div v-if="!$v.emailInput.email" class="check tip-color min-font">邮箱格式不正确</div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">>
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#password"></use>
                    </svg>
                </div>
                <input v-model.trim="passwordInput" @focus="isFocus" @blur="isBlur" type="password" class="inputbox transparent inline-block vertical-align" placeholder="密码(不少于八位)" v-show="!showPass">
                <input v-model.trim="passwordInput" @focus="isFocus" @blur="isBlur" type="text" class="inputbox transparent inline-block vertical-align" placeholder="密码(不少于八位)" v-show="showPass">
                <div class="iconbox full-height eye inline-block vertical-align" v-on:click="showPass = !showPass">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#eye"></use>
                    </svg>
                </div>
            </div>
            <div class="check tip-color min-font" v-if="!$v.passwordInput.minLength">密码请勿少于八位</div>
        </div>
        <div class="row">
            <div class="box box-height transparent">
                <div class="iconbox full-height width inline-block vertical-align">
                    <svg viewBox="0 0 200 200" class="icon">>
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#password"></use>
                    </svg>
                </div>
                <input v-model.trim="psdsecond" @focus="isFocus" @blur="isBlur" class="inputbox transparent inline-block vertical-align" type="password" placeholder="再次输入密码" v-show="!showPass">
                <input v-model.trim="psdsecond" @focus="isFocus" @blur="isBlur" class="inputbox transparent inline-block vertical-align" type="text" placeholder="再次输入密码" v-show="showPass">
                <div class="iconbox full-height eye inline-block vertical-align" v-on:click="showPass = !showPass">
                    <svg viewBox="0 0 200 200" class="icon">
                        <use xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#eye"></use>
                    </svg>
                </div>
            </div>
            <div class="check tip-color min-font" v-if="!$v.psdsecond.sameAs && this.psdsecond">密码输入不一致</div>
        </div>
        <button v-on:click="submit" class="change full-width box-height margin-bottom" :style="{'background-color': this.submitFlag ? '#d2d2d2' : '#fd860e'}" id="registerbtn">注册</button>
    </div>
</template>
<script>
import Input from './Input.vue'
import userInput from './userInput.vue'
import {
    email,
    minLength,
    sameAs,
    required
} from 'vuelidate/lib/validators'
import Service from "../service"
export default {
    data() {
            return {
                username: '',
                emailInput: '',
                passwordInput: '',
                psdsecond: '',
                showPass: false,
                submitFlag: false
            }
        },
        components: {
            "eInput": Input,
            "userInput": userInput
        },
        validations: {
            username: {
                required,
                isUnique(value) {
                    return new Promise((resolve, reject) => {
                        Service.checkUsername(value).then(res=> {
                            resolve(res.ok)
                        },() => {
                            resolve(res.ok)
                        })
                    })
                }
            },
            emailInput: {
                email,
                required,
                isUnique(value) {
                    return new Promise((resolve, reject) => {
                        Service.checkEmail(value).then(res => {
                            resolve(res.ok)
                        },() => {
                            reject(res.ok)
                        })
                    })
                }
            },
            passwordInput: {
                minLength: minLength(8),
                required
            },
            psdsecond: {
                sameAs: sameAs('passwordInput'),
                required
            },
            validationGroup: ['username', 'emailInput', 'passwordInput', 'psdsecond']
        },
        created() {
            this.$parent.footer_display = false
        },
        methods: {
            isFocus() {
                this.submitFlag = false
                this.$parent.footer_display = true
            },
            isBlur() {
                this.$parent.footer_display = false
            },
            isFocus() {
                this.submitFlag = false
                this.$parent.footer_display = true
            },
            submit() {
                if (this.submitFlag) return
                this.submitFlag = true
                if (this.$v.username.required && this.$v.username.isUnique 
                    && this.$v.emailInput.required && this.$v.emailInput.email && this.$v.emailInput.isUnique
                    && this.$v.passwordInput.required && this.$v.passwordInput.minLength
                    && this.$v.psdsecond.sameAs
                    ) {
                    Service.register(this.emailInput, this.username, this.passwordInput).then(res => {
                        if (res !== null && res !== undefined) {
                            window.location = "/"
                        }
                    })
                }
            }
        }
}
</script>