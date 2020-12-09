<template>
<div class='box'>
   <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    default-expand-all
    :tree-props="{children: 'children', hasChildren: 'hasChildren'}">
    <el-table-column
      prop="id"
      label="菜单编号"
      sortable
      width="180">
    </el-table-column>
    <el-table-column
      prop="title"
      label="菜单名称"
      sortable
      width="180">
    </el-table-column>
    <el-table-column
      prop="icon"
      label="菜单图标">
    </el-table-column>
     <el-table-column
      prop="url"
      label="菜单地址">
    </el-table-column>
  <el-table-column
      prop="status"
      label="状态">
     <template slot-scope="scope">
        <el-button type='primary' v-if='scope.row.status == 1'
        >启用</el-button>
        <el-button
          type="danger" v-else
          >禁用</el-button>
      </template>
    </el-table-column>

        <el-table-column
      prop="address"
      label="操作">
     <template slot-scope="scope">
        <el-button type='primary'
          @click="edit(scope.row.id)">编辑</el-button>
        <el-button
          type="danger"
          @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>
    
  </el-table>
</div>
</template>
<script>
import {mapGetters,mapActions} from 'vuex';
import {reqMenuDel} from '../../../util/request'
export default {
   computed:{
       ...mapGetters({
           list:"menu/list",
       })
   } ,
components:{
},
 data() {
      return {

      }
    },
    methods: {
       ...mapActions({
        requestMenuList:'menu/requestMenuList',
    }),
//     handleEdit(scope){
// console.log(scope)
//     },
//     handleDelete(){

//     }
  edit(id){
    this.$emit('edit',id)
  },
  del(id){
    reqMenuDel({id:id}).then(res=>{
          alert('删除成功')
          this.requestMenuList()
      })
  }
    },
mounted() {
this.requestMenuList();
}
}
</script>
<style scoped>
.box{
    margin-top:20px;
}
</style>