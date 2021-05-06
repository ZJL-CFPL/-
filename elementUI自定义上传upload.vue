<template>
  <div class="upload-box" v-loading="loading">
    <el-upload
      class="upload-demo"
      action
      ref="upload"
      :http-request="httpRequest"
      multiple
      :limit="5"
      :on-progress="fileProgress"
      :on-exceed="handleExceed"
      :on-change ='fileChange'
      :auto-upload="false"
      :on-success="uploadSuccess" 
      :file-list="fileList">
      <el-button slot="trigger" size="small" type="primary">点击上传</el-button>
      <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>
      <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
      <a href="https://view.officeapps.live.com/op/view.aspx?src=http://upload.aukeyit.com/quality/develop/test_a5b064d67b154c33a6c3ff3f61aaf646.docx" target="_blank">test</a>
      <div class="fileslot" slot="file" slot-scope="{file}">
        <span class="file-operate file-href">{{ file.name }}</span>
        <a v-if="file.url" class="file-operate file-download" :href="file.url" target="_blank">下载</a>
        <span class="file-operate file-remove" @click="handleRemove(file)">删除</span>
        <span class="file-operate file-reupload" @click="handleReUpload(file)">重新上传</span>
      </div>
    </el-upload>
  </div>
</template>
<script>
import { uploadFiles } from './resquest'
export default {
    data() {
      return {
        loading: false,
        fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}],
        reUploadId: ''
      };
    },
    methods: {
      httpRequest (fileObj) {
        console.log('上传请求')
        let formData = new FormData();
        formData.set("file", fileObj.file);
        uploadFiles(formData).then(res => {
          if(res.status === 200 && res.data.code === 0){
            alert('上传成功')
          }
        })
      },
      handleRemove(file) {
        this.$confirm('此操作将删除该文件, 是否继续?', '', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.fileList = this.fileList.filter(item=>{
            return item.uid != file.uid
          })
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      handleExceed(files, fileList) {
        this.$message.warning(`当前限制选择 5 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      handleReUpload(file){
        this.reUploadId = file.uid
        this.$refs['upload'].$children[0].$refs.input.click()
      },
      fileProgress(){
      },
      fileChange(file, fileList){
        if(this.reUploadId){
          const findReUploadIndex = (ele) => ele.uid == this.reUploadId;
          const reUploadIndex = fileList.findIndex(findReUploadIndex)
          let currentList = fileList.filter(item => {
            return item.uid != this.reUploadId
          })
          currentList.splice(reUploadIndex, 0 , file)
          currentList.pop()
          this.fileList = currentList
          this.reUploadId = ''
        }else{
          this.fileList = fileList
        }
      },
      submitUpload() {
        console.log(this.fileList)
        // return
        this.$refs.upload.submit();
      },
      uploadSuccess(){
        // 有个小问题，上传事件只会触发一次，所以需要在组件上传完成后的钩子里将上传文件列表清除掉 (----待验证)
        // this.$refs.upload.clearFiles()
      }
    }
  }
</script>

<style lang="scss">
  .upload-box{
    padding: 20px;
  }
  .file{
    &-operate{
      display: inline-block;
      vertical-align: middle;
      padding: 4px 6px;
      margin-right: 6px;
      cursor: pointer;
      color: #1995ff;
      &:hover{
        text-decoration: underline;
      }
    }
    &-href{
      text-decoration: underline;
      padding-left: 0;
    }
  }
</style>
