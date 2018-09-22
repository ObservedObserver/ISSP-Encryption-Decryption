<template>
  <div>
    <el-container>
      <el-header style="background-color: #545c64">
        <el-menu 
        background-color="#545c64"
        default-active="1"
        mode="horizontal"
        text-color="#fff"
        active-text-color="#ffd04b">
          <el-menu-item index="1">Assignment 2</el-menu-item>
        </el-menu>
      </el-header>
      <el-container style="padding: 1rem 2rem">
        <el-aside width="260px">
          <el-menu class="el-menu-vertical-demo"
            @select="handleSelect"
            :default-active="leftMenu[0].value"
            text-color="#545c64"
            active-text-color="#ffd04b">
            <el-menu-item v-for="item in leftMenu" :key="item.value" :index="item.value">
              <i class="el-icon-menu"></i>
              <span slot="title">{{item.label}}</span>
            </el-menu-item>
          </el-menu>
        </el-aside>
        <el-main>
          <ciphers :method="currentAlg" />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import ciphers from './ciphers/index.vue'
export default {
  name: 'app-index',
  data () {
    return {
      currentAlg: '1',
      leftMenu: [
        {label: 'Substitution Ciphers', value: '1'},
        {label: 'Transposition Ciphers', value: '2'},
        {label: 'Product Ciphers', value: '3'}
      ]
    }
  },
  methods: {
    handleSelect (index) {
      this.currentAlg = index
    }
  },
  components: {
    ciphers
  }
}
</script>
<style>

</style>

