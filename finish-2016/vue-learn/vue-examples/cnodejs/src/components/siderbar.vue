<template>
    <div class="siderbar">
        <div class="panel" v-if="$route.name === 'post' || $route.name === 'user' || token">
            <div class="panel-header">
                个人信息
            </div>
            <div class="inner padding">
                <div class="user-info">
                    <a v-link="{name:'user',params:{name:user.loginname}}" class="user-logo">
                        <img :src="user.avatar_url" alt="avatar">
                    </a>
                    <a v-link="{name:'user',params:{name:user.loginname}}" class="user-name">{{user.loginname}}</a>
                     <div class="user-score">积分：{{ user.score }}</div>
                </div>
            </div>
        </div>
        <div class="panel" v-if="token">
            <div class="inner padding">
                <a v-link="{name:'create'}" class="btn btn-success">发布话题</a>
            </div>
        </div>
        <div class="panel" v-if="$route.name !=='post' && $route.name !== 'user' && !token">
            <div class="panel-header">CNODE:node.js专业中文社区</div>
            <div class="inner padding">
                <div class="sign-about">您可以通过accessToken登入</div>
                <a href="#" class="btn btn-primary" @click.prevent.stop="preLogin">通过token登入</a>
            </div>
        </div>
    </div>
</template>
<script>
    import {getToken,getUser} from '../vuex/getters';
    import { changeLoginUser, changeToken, checkToken, fetchMsgCount, fetchCollection, fetchUser } from '../vuex/actions';

    export default{
        vuex:{
            getters:{
                token:getToken,
                user:getUser
            },
            actions: {
                changeLoginUser,
                changeToken,
                checkToken,
                fetchCollection,
                fetchMsgCount,
                fetchUser,
            }
        },
        methods:{
            preLogin(){
                if(document.cookie.length > 0){

                    const arr = document.cookie.split(';');
                    let t;
                    for(let v of arr){
                        v.trim();
                        if(v.startsWith('token=')){
                            t = v.split('=')[1];
                            break;
                        }
                    }
                    if(t){


                        this.changeToken(t);
                        this.checkToken(t)
                            .then(this.fetchUser)
                            .then((info) => {
                                this.changeLoginUser(info);
                                return info.loginname;
                            })
                            .then((name) => this.fetchCollection(name))
                            .then(() => this.fetchMsgCount(this.token))
                            .then(() => {
                                this.$route.router.go({
                                    name:'index'
                                })
                            })
                            .catch(() => {
                                this.$route.router.go({
                                    name:'login'
                                });
                            }) ;
                    }else{
                        this.$route.router.go({
                            name:'login'
                        });
                    }
                }else{
                    this.$route.router.go({
                        name:'login'
                    });
                }
            }
        }
    }
</script>

<style lang="less">
    .user-info{
        font-size: 15px;
        img{
            width: 48px;
            height: 48px;
            margin-bottom: 10px;
        }
    }

    .user-logo{
        margin-right: 8px;
    }

    .user-name{
        color:#778087;
    }

    .user-score{
        margin:10px 0;
    }

    .user-signature{
        font-size:13px;
        font-style:italic;
        margin-bottom: 8px;
    }

    .sign-about{
        margin-bottom: 10px;
        font-size: 14px;
    }

    .padding{
        padding:10px;
    }

    .btn{
        color:#fff;
        padding:8px;
        border-radius: 4px;
        display: inline-block;
        transition:all 0.2s;
    }

    .btn-success{
        background-color: #80bd01;
        &:hover{
            background-color: darken(#80bd01,10%);
        }
    }

    .btn-primary{
        background-color: #5bc0de;
        &:hover{
            background-color: darken(#5bc0de,10%);
        }
    }
</style>

