<template>
<div>
    <el-button type='primary' @click="willAdd">添加</el-button>
  <v-add :info='info' ref="one"></v-add>
  <v-list @edit='edit($event)'></v-list>
  </div>


</template>
<script>
import vAdd from './components/add'
import vList from './components/list'
export default {
components:{
    vAdd,
    vList,
},
data(){
return{
  info:{
    isShow:false,
    isAdd:true
  }
}
},
methods:{
  //点击添加
   willAdd(){
    this.info.isShow=!this.info.isShow
    this.info.isAdd =true
    this.info.title="菜单添加"
  },
  //点击修改
  edit(e){
this.info.isShow=true
this.info.isAdd = false
    this.info.title="菜单修改"

this.$refs.one.look(e)
  }
},
mounted() {

}
}
</script>
<style scoped>
</style>