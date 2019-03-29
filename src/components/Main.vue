<template>
<div>
    <h1 v-if="firstview">🔍输入用户名搜索</h1>
    <h1 v-if="loading">玩命加载中💗</h1>
    <h1 v-if="errorMsg">{{errorMsg}}</h1>
  <div class="row">
      <div  :key="index" v-for="(user,index) in users" class="card">
        <a :href="user.url" target="_blank">
          <img :src='user.avatar_url' style='width: 100px'/>
        </a>
        <p class="card-text">{{user.name}}</p>
      </div>
    </div>
    </div>
</template>

<script>
import PubSub from 'pubsub-js'
import axios from 'axios'
export default {
  created(){
      //订阅消息
      PubSub.subscribe('search',(msg,searchname)=>{
         //说明需要发送ajax请求搜索
         const url=`https://api.github.com/search/users?q=${searchname}`
          //发ajax请求
         
          //发送请求时候要更新状
          this.firstview = false
          this.loading = true
          this.users = ''
          this.errorMsg = ''
          axios.get(url).then(response=>{
              const result = response.data
              const users = result.items.map(item=>{
                return {
                url:item.html_url,
                avatar_url:item.avatar_url,
                name:item.login
                }  
              })
     //成功，更新状态（成功）
                    
            this.loading = false
            this.users = users
             }).catch(error=>{
                   this.loading = false
                   this.errorMsg = '请求失败'
             }) 
               
            //失败，更新状态（失败）
      })
  },
  data(){
   return {
       firstview:true,
       loading:false,
       users:null,
       errorMsg:"", //[{url:"",avatar_url:'',name:''}]

   }
  }
}
</script>

<style scoped>
.card {
  float: left;
  width: 33.333%;
  padding: .75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: .75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>
