<template>
  <div class="fileBox">
    <input ref="file-upload-input" class="file-upload-input" type="file" accept=".jpg,.png,.bmp,.jpeg"
      @change="handleClick">
    <div class="drop" @drop="handleDrop" @dragover="handleDragover" @dragenter="handleDragover">
      Drag or click to upload
      <el-button :loading="loading" style="margin-left:16px;" size="mini" type="primary" @click="handleUpload">
        Upload
      </el-button>
    </div>

  </div>
</template>

<script>

export default {
  name: 'UploadFile',
  props: {
    beforeUpload: Function, // eslint-disable-line
    onSuccess: Function// eslint-disable-line
  },
  data() {
    return {
      uploadForm: {
        fileData: ''
      },
      loading: false,
      excelData: {
        header: null,
        results: null
      }
    }
  },
  methods: {
    generateData({ header, results }) {
      this.excelData.header = header
      this.excelData.results = results
      this.onSuccess && this.onSuccess(this.excelData)
    },
    handleDrop(e) {
      e.stopPropagation()
      e.preventDefault()
      if (this.loading) return
      const files = e.dataTransfer.files
      if (files.length !== 1) {
        this.$message.error('Only one file can be uploaded')
        return
      }
      const rawFile = files[0] // only use files[0]

      if (!this.isfile(rawFile)) {
        this.$message.error('Please upload correct image file')
        return false
      }
      this.upload(rawFile)
      e.stopPropagation()
      e.preventDefault()
    },
    handleDragover(e) {
      e.stopPropagation()
      e.preventDefault()
      e.dataTransfer.dropEffect = 'copy'
    },
    handleUpload() {
      this.$refs['file-upload-input'].click()
    },
    handleClick(e) {
      const files = e.target.files
      const rawFile = files[0] // only use files[0]
      if (!rawFile) return
      this.upload(rawFile)
    },
    upload(rawFile) {
      this.$refs['file-upload-input'].value = null // fix can't select the same excel

      if (!this.beforeUpload) {

        this.$message({
          duration: 3000,
          message: 'Uploaded image successfully!',
          type: 'success'
        })

        this.readerData(rawFile)
        return
      }
      const before = this.beforeUpload(rawFile)
      if (before) {
        this.readerData(rawFile)
      }
    },
    readerData(rawFile) {
      this.loading = true
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.onload = e => {
          console.log('reader.onload = e => ========================', e)
          console.log('reader.onload = reject => ========================', reject)

          // this.imageDataUrl = e.target.result; // 将图片数据存储在 imageDataUrl 中


          // var imageObj = new Image();
          // imageObj.src = e.target.result;
          // let imageObjW;
          // let imageObjH;
          // imageObj.onload = function () {
          //   console.log("imageObj.width = " + imageObj.width);
          //   console.log("imageObj.height = " + imageObj.height);
          //   imageObjW = imageObj.width;
          //   imageObjH = imageObj.height;
          //   // return [imageObj.width, imageObj.height]
          // };

          // console.log("out imageObjW = " + imageObjW);
          // console.log("out imageObjH = " + imageObjH);

          function imageWH(src) {
            return new Promise((resolve, reject) => {
              let imageObj = new Image();
              imageObj.onload = function () {
                resolve({
                  width: imageObj.width,
                  height: imageObj.height
                });
              };
              imageObj.onerror = function () {
                reject(new Error('Image failed to load'));
              };
              imageObj.src = src;
            });
          }

          imageWH(e.target.result)
            .then(({ width, height }) => {
              console.log('Image loaded with width:', width, 'and height:', height);
              // 你可以在这里使用width和height
              this.$emit('image-upload', e.target.result, width, height);
              console.log("emit width = " + width);
              console.log("emit height = " + height);
            })
            .catch(error => {
              console.error(error);
            });


          // this.$emit('image-upload', e.target.result, imageObj.width, imageObj.height);

          this.loading = false
          resolve()
        }
        // 读取文件为 DataURL (base64 编码)
        reader.readAsDataURL(rawFile);
        // reader.readAsArrayBuffer(rawFile)
      })
    },
    isfile(file) {
      return /\.(jpg|png|jpeg|bmp)$/.test(file.name)
    }
  }
}
</script>

<style scoped>
.noteFontfile {
  color: rgb(10, 2, 2);
  /* background-color: rgb(0, 20, 56); */
}

.fileBox {
  color: aqua;
  /* background-color: rgba(192, 220, 223, 0.6); */
  height: 39px;
  margin: auto 0;
  margin-top: 0;
  display: block;
}

.file-upload-input {
  display: none;
  z-index: -9999;
}

.drop {
  border: 1px dashed #bbb;
  width: 1000px;
  height: 100%;
  margin: 0 auto;
  font-size: 20px;
  border-radius: 5px;
  text-align: center;
  color: #05160ccc;
  padding-top: 10px;
  background: rgb(222, 241, 231);
}
</style>
