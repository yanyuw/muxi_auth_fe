<template>
    <div class="wrap">
        <div class="no-footer">
            <div class="header text-align full-width">
                <div class="logo"></div>
            </div>
            <div class="full-height full-width main">
                <div class="top">
                    <div class="circle margin"></div>
                </div>
                <div class="bottom full-width">
                    <div class="container margin">
                        <div class="text-align router-margin">
                            <!-- <router-link to="/phone" class="btn"> -->
                                <router-link to="/" class="btn">
                                <a v-on:click="tologin" v-bind:class="{highlight: login}">登录</a>
                            </router-link>
                            <router-link to="/register" class="registerButton btn">
                                <a v-on:click="toregister" v-bind:class="{ highlight: !login}">注册</a>
                            </router-link>
                        </div>
                        <router-view></router-view>
                    </div>
                </div>
            </div>
        </div>
        <div class="footer-copy full-width" v-bind:class="{display: footer_display || email_focus || username_focus}">
            <div class="footer full-width">
                <a href="https://xueer.muxixyz.com" class="word word-change text-align">学而</a>
                <a href="http://muxistudio.com" class="word word-change text-align">木犀团队</a>
                <a href="https://ccnubox.muxixyz.com" class="word word-change text-align">华师匣子</a>
            </div>
            <div class="full-width">
                <div class="copyright text-align">
                    华中师范大学木犀团队
                </div>
                <div class="copyright text-align">
                    Hello from Wuhan，2014-2017 MuxiStudio
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Login from './login.vue'
import Register from './register.vue'
export default {
    data() {
            return {
                login: true,
                footer_display: false,
                email_focus: false,
                username_focus: false
            }
        },
        components: {
            "login": Login,
            "register": Register
        },
        methods: {
            tologin: function() {
                this.login = true
            },
            toregister: function() {
                this.login = false
            }
        }
}
</script>
<style lang="scss">
@import '../mobile.scss';
.router-margin {
    margin-bottom: 10px;
}
.display {
    display: none;
}
.highlight {
    color: black;
}

.btn {
    font-size: 14px;
    border: none;
    color: grey;
}

.registerButton {
    margin-left: 20px;
}

.container {
    font-size: 18px;
    color: rgba(71, 70, 68, 0.99);
    width: 72%;
}

.box-height {
    height: 34px;
}

.inputbox-width {
    width: 66%;
}

.eye {
    float: right;
    width: 10%;
}

.margin-bottom {
    margin-bottom: 100px;
}
</style>
