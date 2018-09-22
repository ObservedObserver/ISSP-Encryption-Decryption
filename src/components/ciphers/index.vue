<template>
  <div>
    <el-radio-group v-model="mode" style="margin-bottom: 30px;">
      <el-radio-button label="encrypt">encrypt</el-radio-button>
      <el-radio-button label="decrypt">decrypt</el-radio-button>
    </el-radio-group>
    <substitution v-if="method === '1' " :mode="mode" />
    <transposition v-if="method === '2' " :mode="mode" />
    <product v-if="method === '3' " :mode="mode" />
  </div>  
</template>
<script>
import product from './product/encrypt.vue'
import substitution from './substitution/encrypt.vue'
import transposition from './transposition/encrypt.vue'
export default {
  name: 'ciphers',
  props: {
    method: {
      type: String,
      default: '1'
    }
  },
  data () {
    return {
      mode: 'encrypt'
    }
  },
  components: {
    substitution,
    transposition,
    product
  }
}
</script>
<style>

</style>
