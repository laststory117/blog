<template>
  <el-form @submit.native.prevent="editArticle" ref="form" :model="article" label-width="80px">
    <el-form-item label="文章标题">
      <el-input v-model="article.title"></el-input>
    </el-form-item>
    <el-form-item label="文章内容">
      <el-input type="textarea" style="margin-bottom:20px" v-model="article.body"></el-input>
      <tinymce-editor ref="editor"
        v-model="msg"
        :disabled="disabled"
        @onClick="onClick">
      </tinymce-editor>
      <el-button style="margin:20px 10px" @click="clear">清空内容</el-button>
      <el-button style="margin:20px 10px" @click="disabled = true">禁用</el-button>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" native-type="submit">保存</el-button>
      <el-button @click="$router.go(-1)">取消</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
import TinymceEditor from '@/components/tinymce-editor'
export default {
  components: {
    TinymceEditor
  },
  data () {
    return {
      article: {},
      msg: 'Welcome to Use Tinymce Editor',
      disabled: false
    }
  },
  methods: {
    editArticle () {
      this.$api.editArticle(this.$route.params.id, this.article).then(() => {
        this.$api.getUserAll().then(res => {
          this.$message({
            message: '文章更新成功',
            type: 'success'
          })
          this.$router.push('/articles/index')
        })
      })
    },
    fetch () {
      this.$api.getArticle(this.$route.params.id).then(res => {
        if (res.code === 1000) {
          this.article = res.data
        }
      })
    },
    // 鼠标单击的事件
    onClick (e, editor) {
      console.log('Element clicked')
      console.log(e)
      console.log(editor)
    },
    // 清空内容
    clear () {
      this.$refs.editor.clear()
    }
  },
  created () {
    this.fetch()
  }
}
</script>
