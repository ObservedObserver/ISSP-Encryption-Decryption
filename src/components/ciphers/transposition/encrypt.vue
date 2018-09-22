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
      <div class="table-container">
        <table class="trans-table">
          <tr v-for="(row, i) in encryptTable" :key="i">
            <td v-for="(item, j) in row" :key="j">{{item}}</td>
          </tr>
        </table>
      </div>
      <h4>{{showInfo.transTitle}}</h4>
      <p>{{encryptText}}</p>
    </div>
  </el-card>
</template>

<script>
const BLANK_BLOCK = '-null-'
export default {
  name: 'transposition-encrypt',
  props: {
    mode: {
      type: String,
      default: 'decrypt'
    }
  },
  data () {
    return {
      form: {
        key: '1',
        plainText: ''
      },
      showInfo: {
        title: 'Transposition Encryption',
        originTitle: 'Plain Text',
        transTitle: 'Encrypt Text'
      }
    }
  },
  watch: {
    mode (val) {
      if (val === 'encrypt') {
        this.showInfo = {
          title: 'Transposition Encryption',
          originTitle: 'Plain Text',
          transTitle: 'Encrypt Text'
        }
      } else {
        this.showInfo = {
          title: 'Transposition Decryption',
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
          return Math.ceil(this.form.plainText.length / parseInt(this.form.key))
        }
      } else {
        return 'WRONG_TYPE_KEY'
      }
    },
    encryptMatrix () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return this.encryptKey
      }
      let key = this.encryptKey
      let ans = []
      for (let i = 0; i < key; i++) {
        ans.push([])
        for (let j = 0; j < i; j++) {
          ans[i].push(BLANK_BLOCK)
        }
      }
      let plainText = this.form.plainText
      for (let i = 0; i < plainText.length; i++) {
        ans[i % key].push(plainText[i])
      }
      return ans
    },
    encryptText () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return this.encryptKey
      }
      let ans = this.encryptMatrix
      return ans.map(row => {
        return row.filter(char => {
          return char !== BLANK_BLOCK
        }).join('')
      }).join('')
    },
    encryptTable () {
      if (this.encryptKey === 'WRONG_TYPE_KEY') {
        return []
      }
      let matrix = this.encryptMatrix
      return matrix.map(row => {
        return row.map(char => {
          return char === BLANK_BLOCK ? ' ' : char
        })
      })
    }
  }
}
</script>

<style>
.table-container{
  width: 100%;
  overflow: auto;
}
.trans-table > tr > td{
  padding: 10px;
  border: 1px solid #aaa;
}
</style>
