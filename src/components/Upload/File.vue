<template>
  <div>
    <el-upload
      :action="action"
      :limit="limit"
      :headers="headers"
      :file-list="fileList"
      :disabled="disabled"
      :style="{'width': width+'px'}"
      :multiple="multiple"

      :before-remove="beforeRemove"
      :on-success="handleSuccess"
      :on-error="handleError"
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :on-exceed="handleExceed"
    >
      <el-button size="small" type="primary">点击上传</el-button>
      <div slot="tip" class="el-upload__tip">{{ tip }}</div>
    </el-upload>
  </div>
</template>

<script>
export default {
  name: 'FileUpload',
  props: {
    value: {
      type: Array,
      default: () => []
    },
    action: {
      type: String,
      default: ''
    },
    limit: {
      type: Number,
      default: 1
    },
    headers: {
      type: Object,
      default: () => {}
    },
    disabled: {
      type: Boolean,
      default: false
    },
    width: {
      type: Number,
    },
    multiple: {
      type: Boolean,
      default: false
    },
    tip: {
      type: String,
      default: ''
    },
  },
  data () {
    return {
      fileList: this.value
    }
  },
  methods: {
    beforeRemove(file, fileList) {
      return this.$confirm(`确定要移除 ${file.name}？`)
    },
    handleSuccess(response, file, fileList) {
      this.fileList.push({
        uid: file.uid,
        name: file.name,
        percentage: file.percentage,
        status: file.status,
        url: response.url,
      })
    },
    handleError(err, file, fileList) {
      this.$message.error(err.toString())
    },
    handlePreview(file) {
      window.open(file.url, '_blank')
    },
    handleRemove (file, fileList) {
      this.fileList.splice(this.fileList.findIndex(item => item.uid === file.uid), 1)
    },
    handleExceed(files, fileList) {
      this.$message.warning(`最多允许上传 ${this.limit} 个文件。`)
    },
  }
}
</script>
