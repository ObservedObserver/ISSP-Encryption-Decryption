<template>
    <div>
      <el-radio-group v-model="mode" style="margin-bottom: 30px;">
      <el-radio-button label="encrypt">encrypt</el-radio-button>
      <el-radio-button label="decrypt">decrypt</el-radio-button>
    </el-radio-group>
    <encrypt :mode="mode" />
  </div>  
</template>
<script>
import encrypt from './encrypt.vue'
export default {
  name: 'product',
  data () {
    return {
      mode: 'encrypt'
    }
  },
  components: {
    encrypt
  }
}
</script>
<style>

</style>
