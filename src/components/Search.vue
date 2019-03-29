<template>
 <section class="jumbotron">
      <h3 class="jumbotron-heading">🏠请输入你要搜索的github用户</h3>
      <div>
        <input  v-model='searchname' type="text" placeholder="enter the name you search"/>
        <button  @click="search" >Search</button>
      </div>
    </section>
</template>

<script>
import PubSub from 'pubsub-js'
export default {

  data(){
   return {
       searchname:""
   }
  },
  methods:{
      search(){
          const searchname = this.searchname.trim()
          if(searchname){
                //发布搜索的消息
                PubSub.publish('search',searchname)
          }

      }
  }
}
</script>

<style scoped>

</style>
