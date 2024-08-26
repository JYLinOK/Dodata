<script setup>
import UploadFile from './components/UploadFile/index.vue'
</script>

<template>

  <div class="bigBox">
    <h2>DoData-Pixel partition data set making</h2>

    <!-- <p>imageData = {{ imageData }}</p>
    <p>this.imageData = {{ this.imageData }}</p>
    <p>imageDataUrl = {{ imageDataUrl }}</p>
    <p>this.imageDataUrl = {{ this.imageDataUrl }}</p>
    <p>btnWidth = {{ btnWidth }}</p>
    <p>show = {{ show }}</p> -->

    <br>
    <br>
    Upload the reference image:
    <br>
    <UploadFile v-if=true @image-upload="handleImageUploadRef" ref='UploadFile1' v-model="file1" />
    <br>
    <br>
    <div v-if=imageData>
      <p>Reference Image: {{ imageRefWidth }} x {{ imageRefHeight }}</p>
      <img :src=imageData alt="Reference Image" />
    </div>

    <br>
    <br>
    Upload the pixel image:
    <br>
    <UploadFile v-if=true @image-upload="handleImageUploadPixel" ref='UploadFile2' v-model="file2" />
    <br>
    <br>
    <div v-if=imageDataPixel>
      <p>Pixel Image: {{ imagePixelWidth }} x {{ imagePixelHeight }}</p>
      <img :src=imageDataPixel alt="Pixel Image" />
    </div>

    <br>
    <br>

    <hr>

    <span>
      Reference Image=
      <el-input-number v-model="imageRefWidth" :precision="1" :step="0.1" /> x
      <el-input-number v-model="imageRefHeight" :precision="1" :step="0.1" />

      &nbsp;&nbsp;&nbsp; Pixel Image=
      <el-input-number v-model="imagePixelWidth" :precision="1" :step="0.1" /> x
      <el-input-number v-model="imagePixelHeight" :precision="1" :step="0.1" />
    </span>

    <br>
    <br>
    <span>
      Ref zoom:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="zoomRef" show-input :precision="1" :step="1" :min="1" :max="10"
          @change="changZoomRef" />
      </div>
    </span>
    <span>
      Pixel zoom:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="zoomPixel" show-input :precision="1" :step="1" :min="1" :max="10"
          @change="changeZoomPixel" />
      </div>
    </span>

    <span>
      Ref Image width:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="imageRefWidth" show-input :precision="1" :step="1"
          :max="imageRefWidthPre * zoomRef" />
      </div>
    </span>
    <span>
      Ref Image height:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="imageRefHeight" show-input :precision="1" :step="1"
          :max="imageRefHeightPre * zoomRef" />
      </div>
    </span>

    <span>
      Pixel Image width:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="imagePixelWidth" show-input :precision="1" :step="1"
          :max="imagePixelWidthPre * zoomPixel" />
      </div>
    </span>
    <span>
      Pixel Image height:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="imagePixelHeight" show-input :precision="1" :step="1"
          :max="imagePixelHeightPre * zoomPixel" />
      </div>
    </span>

    <span>
      Ref Image Opacity:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="opacityRef" show-input :precision="1" :step="0.1" :max="1" />
      </div>
    </span>
    <span>
      Pixel Image Opacity:
      <div class="slider-block">
        <el-slider class="self_slider" v-model="opacityPixel" show-input :precision="1" :step="0.1" :max="1" />
      </div>
    </span>

    <span>
      Ref Image Up=
      <el-switch v-model="refUp" @change="upImageRef" />
      &nbsp;&nbsp;&nbsp; Pixel Image Up=
      <el-switch v-model="PixelUp" @change="upImagePixel" />
    </span>


    <br>
    <br>
    <br>

    <img v-if=imageData class="imgBoxRef"
      :style="{ width: imageRefWidth + 'px', height: imageRefHeight + 'px', opacity: opacityRef, zIndex: zIndexRef }"
      :src=imageData alt="Reference Image" />
    <br>
    <img v-if=imageDataPixel class="imgBoxPixel"
      :style="{ width: imagePixelWidth + 'px', height: imagePixelHeight + 'px', opacity: opacityPixel, zIndex: zIndexPixel }"
      :src=imageDataPixel alt="Pixel Image" />



    <br>
    <br>
    <br>


    <el-button @click="addItem" type="primary">Add</el-button>
    <br>
    <br>

    <div class="canvas">
      <div v-for="row in form.rows" :key="row">
        <span>
          <!-- <div v-for="(item, index) in form.dynamicItem" :key="index"> -->
          <el-button v-for="(item, index) in form.dynamicItem" :key="index" v-bind:style="{ width: btnWidth + 'px' }"
            type="primary">{{ item }}</el-button>
          <!-- </div> -->
        </span>
      </div>
    </div>


    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>



  </div>

</template>

<script>
export default {
  components: {
    UploadFile
  },
  name: 'App',
  data() {
    return {
      imageData: null,
      imageDataPixel: null,
      imageRefWidth: 0,
      imageRefWidthPre: 0,
      imageRefHeight: 0,
      imageRefHeightPre: 0,
      imagePixelWidth: 0,
      imagePixelWidthPre: 0,
      imagePixelHeight: 0,
      imagePixelHeightPre: 0,
      opacityRef: 1,
      opacityPixel: 1,
      zoomRef: 1,
      zoomPixel: 1,
      refUp: false,
      PixelUp: true,
      zIndexRef: 1,
      zIndexPixel: 2,
      file1: "",
      file2: "",
      show: true,
      btnWidth: 10,
      form: {
        dynamicItem: [
          'btn1',
          'btn2',
          'btn3',
          'btn4',
          'btn5',
          'btn5',
          'btn5',
          'btn5',
          'btn5',
          'btn5',
          // 'btn6',
          // 'btn7',
          // 'btn8',
          // 'btn9'
        ],
        rows: 1,
        colums: 10,
      }
    };
  },
  methods: {
    addItem() {
      this.form.dynamicItem.push({
      })
    },
    deleteItem(item, index) {
      this.form.dynamicItem.splice(index, 1)
    },
    handleImageUploadRef(img, width, height) {
      this.imageData = img;
      console.log("this.imageData = " + this.imageData);
      console.log("ref width = " + width);
      console.log("ref height = " + height);
      this.imageRefWidth = width
      this.imageRefWidthPre = width
      this.imageRefHeight = height
      this.imageRefHeightPre = height
    },
    handleImageUploadPixel(img, width, height) {
      this.imageDataPixel = img;
      console.log("this.imageDataPixel = " + this.imageDataPixel);
      console.log("pixel width = " + width);
      console.log("pixel height = " + height);
      this.imagePixelWidth = width
      this.imagePixelWidthPre = width
      this.imagePixelHeight = height
      this.imagePixelHeightPre = height
    },
    upImageRef() {
      this.PixelUp = !this.refUp;
      this.zIndexUp();
    },
    upImagePixel() {
      this.refUp = !this.PixelUp;
      this.zIndexUp();
    },
    zIndexUp() {
      if (this.PixelUp) {
        this.zIndexRef = 2;
        this.zIndexPixel = 1;
      } else {
        this.zIndexRef = 1;
        this.zIndexPixel = 2;
      }
    },
    changZoomRef() {
      this.imageRefWidth = this.imageRefWidth * this.zoomRef
      this.imageRefHeight = this.imageRefHeight * this.zoomRef
    },
    changeZoomPixel() {
      this.imagePixelWidth = this.imagePixelWidth * this.zoomPixel
      this.imagePixelHeight = this.imagePixelHeight * this.zoomPixel
    }
  }
};
</script>

<style scoped>
.bigBox {
  padding: 10px;
  position: fixed;
  margin: auto 0;
  margin-top: 0px;
  background-color: rgb(238, 244, 242);
  width: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  height: 100%;
}

.el-button {
  background-color: #c4ffbc00;
  color: red;
  border-radius: 0%;
  margin: 0px 0px;
  border: 1px solid rgba(4, 223, 139, 0.639);
}

.el-button+.el-button {
  margin-left: 0px;
}

.canvas {
  width: 200px;
  height: 100px;
  background: rgba(147, 223, 209, 0.342);
}

.imgBoxRef {
  position: absolute;
  margin: auto 0;
  background: rgb(0, 96, 128);
  opacity: 1;
  z-index: 2;
}

.imgBoxPixel {
  position: absolute;
  margin: auto 0;
  background: green;
  opacity: 0.7;
  z-index: 1;
}

hr {
  border: 3px solid rgb(129, 129, 192);
}

/* ================================= */

.slider-block {
  max-width: 1000px;
  display: flex;
  align-items: center;
}

.slider-block .el-slider {
  margin-top: 0;
  margin-left: 10px;
}

/* ================================= */
</style>
