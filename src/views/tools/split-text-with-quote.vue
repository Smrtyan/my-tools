<template>
  <div>
    <el-input
      v-model="textarea"
      type="textarea"
      :rows="2"
      placeholder="请输入内容"
    />
    <el-button type="primary" @click="go">Go</el-button>
    <h2>single quote</h2>
    <el-input
      v-model="textareaOutSingle"
      type="textarea"
      :rows="2"
      placeholder=""
    />
    <el-button
      v-clipboard:copy="textareaOutSingle"
      v-clipboard:success="firstCopySuccess"
      v-clipboard:error="firstCopyError"
      type="success"
      size="mini"
      circle
    >Copy</el-button>

    <h2>double quote</h2>

    <el-input
      v-model="textareaOutDouble"
      type="textarea"
      :rows="2"
      placeholder=""
    />
    <el-button
      v-clipboard:copy="textareaOutDouble"
      v-clipboard:success="firstCopySuccess"
      v-clipboard:error="firstCopyError"
      size="mini"
      type="success"
      circle
    >Copy</el-button>
  </div>

</template>

<script>
export default {
  name: 'SplitTextWithQuote',
  data() {
    return {
      textarea: '',
      textareaOutSingle: '',
      textareaOutDouble: ''

    }
  }, methods: {
    go() {
      const text = this.textarea
      const pattern = /("[^"]+")|\b(\w+)\b/g
      const words = []

      text.replace(pattern, function(match, phrase, word) {
        if (phrase) {
          words.push(phrase)
        } else {
          words.push(word)
        }
      })

      this.textareaOutSingle = words.map(x => "'" + x + "'").toString()
      this.textareaOutDouble = words.map(x => '"' + x + '"').toString()
    },
    firstCopySuccess(e) {
      console.log('copy arguments e:', e)
      this.$message('复制成功')
    },
    firstCopyError(e) {
      console.log('copy arguments e:', e)
      this.$message('复制失败')
    }
  }

}
</script>

<style scoped>

</style>
