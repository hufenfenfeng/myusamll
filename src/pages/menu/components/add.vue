<template>
<div>
      <el-dialog :title="info.title" :visible.sync="info.isShow">
  <el-form :model="form">
    <el-form-item label="菜单名称" :label-width="width">
      <el-input v-model="form.title" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="上级菜单" :label-width="width">
      <el-select v-model="form.pid" placeholder="--请选择菜单--" @change="changePid">
        <el-option label="顶级菜单" value="0"></el-option>
      <!-- 动态循环添加数据 菜单分类 -->
 <el-option v-for='item in menuList' :key="item.id" :label="item.title" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
      <el-form-item label="菜单类型" :label-width="width">
         <el-radio v-model="form.type" :label="1" disabled>目录</el-radio>
  <el-radio v-model="form.type" :label="2" disabled>菜单</el-radio>
    </el-form-item>

  <el-form-item label="菜单图标" :label-width="width" v-if="form.type == 1">
      <el-select v-model="form.icon" placeholder="请选择活动区域">
        <el-option label="魔法棒" value="el-icon-magic-stick">
          <i class="el-icon-magic-stick"></i>
        </el-option>
        <el-option label="钥匙" value="el-icon-key">
          <i class="el-icon-key"></i>
        </el-option>
         <el-option label="太阳" value="el-icon-sunny">
          <i class="el-icon-sunny"></i>
        </el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="菜单地址" :label-width="width" v-else>
      <el-select v-model="form.region" placeholder="请选择活动区域">
        <el-option v-for="item in indexRouters" :key="item.path" :label="'/'+item.path" :value="item.name"></el-option>
      </el-select>
    </el-form-item>

    <!-- 状态 -->
  <el-form-item label="状态" :label-width="width">

    <!-- <el-tooltip :content="'Switch value: ' + value" placement="top"> -->
  <el-switch
    v-model="form.status"
    active-color="#13ce66"
    inactive-color="#ff4949"
    :active-value="1"
    :inactive-value="2">
  </el-switch>
<!-- </el-tooltip> -->
    </el-form-item>

  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="info.isShow = false">取 消</el-button>
    <el-button type="primary" @click="add" v-if='info.isAdd'>添 加</el-button>
     <el-button type="primary" @click="update" v-else>修 改</el-button>
    </div>
    </el-dialog>

</div>
</template>
<script>
import { indexRouters } from "../../../router/index";
import {reqMenuAdd, reqMenuListOne,reqMenuEdit} from "../../../util/request"
import {mapGetters,mapActions} from "vuex"
export default {
  props:['info'],
  computed:{
    ...mapGetters({
      menuList:"menu/list"
    })
  },
components:{
},
data(){
return{
  width:'160px',

    // isShow:true,
    form:{
       pid:'',
        title:'',
        type:0,
        icon:'',
        url:'',
        status:1,

    },
    indexRouters
}
},
methods:{
      //  取消弹框
      hide(){
          this.info.isShow=false
      },
      // 重置
    empty(){
      this.form={
        pid: "",
        title: "",
        type: 0,
        icon: "",
        url: "",
        status: 1,
      }
    },
    ...mapActions({
      requestMenuList: "menu/requestMenuList",
    }),
    //  添加操作
    add(){
        reqMenuAdd(this.form).then((res)=>{
            this.hide()
        this.requestMenuList();

        })
    },
    changePid(){
      this.form.type=this.form.pid==0?1:2
    },
    look(id){
      reqMenuListOne({id:id}).then((res)=>{
        this.form=res.data.list
        this.form.id=id
      })
    },
    update(){
      reqMenuEdit(this.form).then((res)=>{
            this.hide()
        this.requestMenuList()
        this.empty()
      });
    }
},
mounted() {

}
}
</script>
<style scoped>
</style>