<script setup>
import { ref } from 'vue'
import { getUserProfile } from '../../../../api/system/user';
import resetPwd from './resetPwd.vue';
import userAvatar from './userAvatar.vue';
import userInfo from './userInfo.vue';

/**
 * 用户信息
 */
const user = ref({})
/**角色信息 */
const roleGroup = ref({})
/**岗位信息 */
const postGroup = ref({})
/**当前选中的tab */
const activeTab = ref("userinfo")

/**获取用户信息 */
const getUser = () => {
   getUserProfile().then(response => {
      user.value = response.data;
      roleGroup.value = response.roleGroup;
      postGroup.value = response.postGroup;
   });
}

getUser();

</script>
<template>
   <div class="app-container">
      <el-row :gutter="20">
         <el-col :span="6" :xs="24">
            <el-card class="box-card">
               <div slot="header" class="clearfix">
                  <span>个人信息</span>
               </div>
               <div>
                  <div class="text-center">
                     <userAvatar :user="user" />
                  </div>
                  <ul class="list-group list-group-striped">
                     <li class="list-group-item">
                        <svg-icon icon-class="user" />用户名称
                        <div class="pull-right">{{ user.userName }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="phone" />手机号码
                        <div class="pull-right">{{ user.phonenumber }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="email" />用户邮箱
                        <div class="pull-right">{{ user.email }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="tree" />所属部门
                        <div
                           class="pull-right"
                           v-if="user.dept"
                        >{{ user.dept.deptName }} / {{ postGroup }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="peoples" />所属角色
                        <div class="pull-right">{{ roleGroup }}</div>
                     </li>
                     <li class="list-group-item">
                        <svg-icon icon-class="date" />创建日期
                        <div class="pull-right">{{ user.createTime }}</div>
                     </li>
                  </ul>
               </div>
            </el-card>
         </el-col>
         <el-col :span="18" :xs="24">
            <el-card>
               <div slot="header" class="clearfix">
                  <span>基本资料</span>
               </div>
               <el-tabs v-model="activeTab">
                  <el-tab-pane label="基本资料" name="userinfo">
                     <userInfo :user="user" />
                  </el-tab-pane>
                  <el-tab-pane label="修改密码" name="resetPwd">
                     <resetPwd :user="user" />
                  </el-tab-pane>
               </el-tabs>
            </el-card>
         </el-col>
      </el-row>
   </div>
</template>

<style lang='scss' scoped>
</style>