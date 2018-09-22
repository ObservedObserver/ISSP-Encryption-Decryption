<template>
  <el-card>
    <div slot="header">
      <span>{{showInfo.title}}</span>
    </div>
    <div>
      <el-form label-position="right" label-width="100px">
        <el-form-item label="Key">
          <el-input v-model="form.key"></el-input>
        </el-form-item>
        <el-form-item label="Key Mode">
          <el-radio v-model="form.keyMode" label="ascii">ASCII</el-radio>
          <el-radio v-model="form.keyMode" label="string">STRING</el-radio>
        </el-form-item>
        <el-form-item :label="showInfo.originTitle">
          <el-input type="textarea" v-model="form.plainText"></el-input>
        </el-form-item>
      </el-form>
      <h4>{{showInfo.transTitle}}</h4>
      <p>{{encryptText}}</p>
    </div>
  </el-card>
</template>

<script>
export default {
  name: 'product-encript',
  props: {
    mode: {
      type: String,
      default: 'decrypt'
    }
  },
  data () {
    return {
      form: {
        key: '0',
        keyMode: 'ascii',
        plainText: ''
      },
      showInfo: {
        title: 'Product Encryption',
        originTitle: 'Plain Text',
        transTitle: 'Encrypt Text'
      }
    } 
  },
  watch: {
    mode (val) {
      if (val === 'encrypt') {
        this.showInfo = {
          title: 'Product Encryption',
          originTitle: 'Plain Text',
          transTitle: 'Encrypt Text'
        }
      } else {
        this.showInfo = {
          title: 'Product Decryption',
          originTitle: 'Encrypt Text',
          transTitle: 'Plain Text'
        }
      }
    }
  },
  computed: {
    encryptKey () {
      if (this.form.keyMode === 'ascii') {
        if (isNaN(parseInt(this.form.key))) {
          return 'WRONG_TYPE_KEY'
        }
        return parseInt(this.form.key) % 256
      } else {
        if (this.form.key.length === 0) {
          return 'WRONG_TYPE_KEY'
        }
        return this.form.key[0].charCodeAt()
      }
    },
    encryptText () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return this.encryptKey
      }
      let key = this.encryptKey
      let ans = ''
      let plainText = this.form.plainText
      for (let i = 0; i < plainText.length; i++) {
        let ascii = plainText[i].charCodeAt() ^ key
        ans += String.fromCharCode(ascii)
      }
      return ans
    }
  }
}
</script>

<style>

</style>
