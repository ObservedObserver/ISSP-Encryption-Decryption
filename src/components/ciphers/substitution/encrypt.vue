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
        <el-form-item :label="showInfo.originTitle">
          <el-input type="textarea" v-model="form.plainText"></el-input>
        </el-form-item>
      </el-form>
      <el-table :data="encryptTable" height="250" border>
        <el-table-column prop="origin" label="origin charactor"></el-table-column>
        <el-table-column prop="trans" label="trans charactor"></el-table-column>
      </el-table>
      <h4>{{showInfo.transTitle}}</h4>
      <p>{{encryptText}}</p>
    </div>
  </el-card>
</template>

<script>
export default {
  name: 'substitution-encript',
  props: {
    mode: {
      type: String,
      default: 'decrypt'
    }
  },
  data () {
    return {
      form: {
        key: 0,
        plainText: ''
      },
      showInfo: {
        title: 'Substitution Encryption',
        originTitle: 'Plain Text',
        transTitle: 'Encrypt Text'
      }
    } 
  },
  watch: {
    mode (val) {
      if (val === 'encrypt') {
        this.showInfo = {
          title: 'Substitution Encryption',
          originTitle: 'Plain Text',
          transTitle: 'Encrypt Text'
        }
      } else {
        this.showInfo = {
          title: 'Substitution Decryption',
          originTitle: 'Encrypt Text',
          transTitle: 'Plain Text'
        }
      }
    }
  },
  computed: {
    encryptKey () {
      if (!isNaN(parseInt(this.form.key))) {
        if (this.$props.mode === 'encrypt') {
          return parseInt(this.form.key)
        } else {
          return -parseInt(this.form.key)
        }
      } else {
        return 'WRONG_TYPE_KEY'
      }
    },
    encryptTable () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return []
      }
      let key = this.encryptKey
      let Acode = 'A'.charCodeAt()
      let Zcode = 'Z'.charCodeAt()
      let ans = []
      for (let i = Acode; i <= Zcode; i++) {
        ans.push({
          origin: String.fromCharCode(i),
          trans: String.fromCharCode((i - Acode + key + 26) % 26 + Acode)
        })
      }
      return ans
    },
    encryptText () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return this.encryptKey
      }
      let key = this.encryptKey
      let ans = ''
      let plainText = this.form.plainText
      for (let i = 0; i < plainText.length; i++) {
        if (/[A-Z]/.test(plainText[i])) {
          let ascii = (plainText[i].charCodeAt() - 'A'.charCodeAt() + key + 26) %26 + 'A'.charCodeAt()
          ans += String.fromCharCode(ascii)
        } else if (/[a-z]/.test(plainText[i])) {
          let ascii = (plainText[i].charCodeAt() - 'a'.charCodeAt() + key + 26) %26 + 'a'.charCodeAt()
          ans += String.fromCharCode(ascii)
        } else {
          ans += plainText[i]
        }
      }
      return ans
    }
  }
}
</script>

<style>

</style>
