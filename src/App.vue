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

    <hr class="bigHr">

    <span>
      Reference Image=
      <el-input-number v-model="imageRefWidth" :precision="1" :step="0.1" /> x
      <el-input-number v-model="imageRefHeight" :precision="1" :step="0.1" />

      &nbsp;&nbsp;&nbsp; Pixel Image=
      <el-input-number v-model="imagePixelWidth" :precision="1" :step="0.1" /> x
      <el-input-number v-model="imagePixelHeight" :precision="1" :step="0.1" />

      &nbsp;&nbsp;&nbsp;Original Ref Image= {{ imageRefWidthPre }} x {{ imageRefHeightPre }}
      &nbsp;&nbsp;&nbsp;Original Pixel Image= {{ imagePixelWidthPre }} x {{ imagePixelHeightPre }}

    </span>

    <br>
    <br>
    <span>
      <div class="slider-block">Ref zoom:&nbsp;
        <el-slider v-model="zoomRef" class="el-slider-zoom" :precision="1" :step="0.1" :min="1" :max="10"
          @change="changZoomRef" />
        &nbsp;&nbsp;&nbsp;
        <el-input-number class="slider-input-number" v-model="zoomRef" :min="0" :step="1" @change="changZoomRef" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Pixel zoom:&nbsp;
        <el-slider v-model="zoomPixel" class="el-slider-zoom" :precision="1" :step="0.1" :min="1" :max="10"
          @change="changeZoomPixel" />
        &nbsp;&nbsp;&nbsp;
        <el-input-number class="slider-input-number" v-model="zoomPixel" :min="0" :step="1" @change="changeZoomPixel" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">Ref Image width:&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="imageRefWidth" show-input :precision="1" :step="1"
          :max="imageRefWidthPre * zoomRef * zoomSlider" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ref Image height:&nbsp;&nbsp;&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="imageRefHeight" show-input :precision="1" :step="1"
          :max="imageRefHeightPre * zoomRef * zoomSlider" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">Pixel Image width:
        <el-slider class="self_slider" v-model="imagePixelWidth" show-input :precision="1" :step="0.1"
          :max="imagePixelWidthPre * zoomPixel * zoomSlider" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pixel Image height:&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="imagePixelHeight" show-input :precision="1" :step="0.1"
          :max="imagePixelHeightPre * zoomPixel * zoomSlider" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">Ref Image Opacity:&nbsp;&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="opacityRef" show-input :precision="1" :step="0.1" :max="1" />
        &nbsp;&nbsp;&nbsp;&nbsp;Pixel Image Opacity:&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="opacityPixel" show-input :precision="1" :step="0.1" :max="1" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">Ref Margin Top pix:&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="marginTopRef" show-input :precision="1" :step="0.1"
          :max="100 * zoomRef" />
        &nbsp;&nbsp;&nbsp;&nbsp;Ref Margin Left pix:&nbsp;&nbsp;&nbsp;
        <el-slider class="self_slider" v-model="marginLeftRef" show-input :precision="1" :step="0.1"
          :max="100 * zoomRef" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">Pixel Margin Top pix:
        <el-slider class="self_slider" v-model="marginTopPixel" show-input :precision="1" :step="0.1"
          :max="100 * zoomPixel" />
        &nbsp;&nbsp;&nbsp;&nbsp;Pixel Margin Left pix:&nbsp;
        <el-slider class="self_slider" v-model="marginLeftPixel" show-input :precision="1" :step="0.1"
          :max="100 * zoomPixel" />
      </div>
    </span>

    <hr>

    <span>
      Pixel Image Up =
      <el-switch v-model="PixelUp" @change="upImageRef" />
      &nbsp;&nbsp;&nbsp; Ref Image Up =
      <el-switch v-model="RefUp" @change="upImagePixel" />
      &nbsp;&nbsp;&nbsp; Show Grids Box =
      <el-switch v-model="showGirdsBox1" @change="changeGirdsBox" />
    </span>

    <hr>

    <span>
      <div class="slider-block">
        Grid Cell WH (x1) =
        <el-slider v-model="gridCellWH" class="el-slider-zoom-grid" :precision="1" :step="1" :min="1"
          :max="30 * zoomPixel" />
        &nbsp;&nbsp;&nbsp; Grid Cell WH (x0.1) = &nbsp;
        <el-input-number class="slider-input-number" v-model="gridCellWH" :min="0" :step="0.1" :max="30 * zoomPixel" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Grid Cell WH (x0.01) = &nbsp;
        <el-input-number class="slider-input-number" v-model="gridCellWH" :min="0" :step="0.01" :max="30 * zoomPixel" />
      </div>

      <div class="slider-block">
        Cells W = &nbsp; <el-input-number v-model="gridNumW" :precision="0" :step="1" :min="1" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        Cells H = &nbsp; <el-input-number v-model="gridNumH" :precision="0" :step="1" :min="1" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        Grid Cell Opacity = <el-slider class="self_slider" v-model="opacityGrids" show-input :precision="1" :step="0.1"
          :max="1" />
      </div>
    </span>

    <hr>

    <span>
      <div class="slider-block">
        Grid Margin Top pix (x1) =
        <el-slider v-model="marginTopGrids" class="el-slider-zoom-grid-pix" :precision="1" :step="1" :min="0"
          :max="200 * zoomPixel" />
        &nbsp;&nbsp;&nbsp; Top pix (x0.1) = &nbsp;
        <el-input-number class="slider-input-number" v-model="marginTopGrids" :min="0" :step="0.1"
          :max="200 * zoomPixel" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Top pix (x0.01) = &nbsp;
        <el-input-number class="slider-input-number" v-model="marginTopGrids" :min="0" :step="0.01"
          :max="200 * zoomPixel" />
      </div>

      <div class="slider-block">
        Grid Margin Left pix (x1) =
        <el-slider v-model="marginLeftGrids" class="el-slider-zoom-grid-pix" :precision="1" :step="1" :min="0"
          :max="200 * zoomPixel" />
        &nbsp;&nbsp;&nbsp; Left pix (x0.1) = &nbsp;
        <el-input-number class="slider-input-number" v-model="marginLeftGrids" :min="0" :step="0.1"
          :max="200 * zoomPixel" />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Left pix (x0.01) = &nbsp;
        <el-input-number class="slider-input-number" v-model="marginLeftGrids" :min="0" :step="0.01"
          :max="200 * zoomPixel" />
      </div>
    </span>

    <hr class="bigHr">
    <br>
    <br>
    <br>


    <hr class="bigHr">


    <br>
    <br>
    <br>

    <img v-if=imageData class="imgBoxRef"
      :style="{ width: imageRefWidth + 'px', height: imageRefHeight + 'px', opacity: opacityRef, zIndex: zIndexRef, marginTop: marginTopRef + 'px', marginLeft: marginLeftRef + 'px' }"
      :src=imageData alt="Reference Image" />
    <br>
    <img v-if=imageDataPixel class="imgBoxPixel"
      :style="{ width: imagePixelWidth + 'px', height: imagePixelHeight + 'px', opacity: opacityPixel, zIndex: zIndexPixel, marginTop: marginTopPixel + 'px', marginLeft: marginLeftPixel + 'px' }"
      :src=imageDataPixel alt="Pixel Image" />

    <div v-if="showGirdsBox" class="gridsBox"
      :style="{ width: gridCellWH * gridNumW + 'px', height: gridCellWH * gridNumH + 'px', zIndex: zIndexGrids, marginTop: marginTopGrids + 'px', marginLeft: marginLeftGrids + 'px' }">

      <div v-for="row in gridNumW" :key="row">
        <!-- <div v-for="row in 1" :key="row"> -->
        <span>
          <div class="gridCell" :data-row="column - 1" :data-column="row - 1" v-for="column in gridNumH" :key="column"
            :style="{ width: gridCellWH + 'px', height: gridCellWH + 'px', opacity: opacityGrids }"
            @contextmenu.prevent="rightBtn($event)" @mouseup="handleMouseUp($event)" @mousedown="handleMouseown($event)"
            @mouseover="handleMouseOver($event)">
          </div>
        </span>
      </div>
    </div>






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
      gridCellWH: 0,
      gridNumW: 0,
      gridNumH: 0,
      opacityRef: 0.5,
      opacityPixel: 1,
      opacityGrids: 0.5,
      zoomRef: 1,
      zoomPixel: 1,
      zoomSlider: 1.5,
      RefUp: true,
      PixelUp: false,
      showGirdsBox: false,
      showGirdsBox1: false,
      zIndexRef: 2,
      zIndexPixel: 1,
      zIndexGrids: 3,
      marginTopRef: 0,
      marginLeftRef: 0,
      marginTopPixel: 0,
      marginLeftPixel: 0,
      marginTopGrids: 0,
      marginLeftGrids: 0,
      file1: "",
      file2: "",
      show: true,
      mouse2Down: false,
      btnWidth: 10,
      addCellD: {},
      group: 'group',
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
      this.RefUp = !this.PixelUp;
      this.PixelUp = !this.RefUp;
      this.zIndexUp();
    },
    upImagePixel() {
      this.PixelUp = !this.RefUp;
      this.RefUp = !this.PixelUp;
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
      this.imageRefWidth = this.imageRefWidthPre * this.zoomRef;
      this.imageRefHeight = this.imageRefHeightPre * this.zoomRef;
    },
    changeZoomPixel() {
      this.imagePixelWidth = this.imagePixelWidthPre * this.zoomPixel;
      this.imagePixelHeight = this.imagePixelHeightPre * this.zoomPixel;
    },
    changeGirdsBox() {
      this.showGirdsBox = !this.showGirdsBox;
      this.zIndexGrids = 3;
    },
    rightBtn(e) {
      // e.preventDefault();
      console.log("e = " + e);
      // console.log("e.clientX = " + e.clientX);
      // console.log("e.clientY = " + e.clientY);
      // // var el = e.currentTarget;
      // var el = e.target;
      // console.log("el = " + el);
      // console.log("el.dataset = " + el.dataset);
      // console.log("el.dataset.row = " + el.dataset.row);
      // console.log("el.dataset.column = " + el.dataset.column);
    },
    handleMouse(e) {
      if (e.button == 0) {
        console.log("点击左键");
      } else if (e.button == 1) {
        console.log("点击滚轮");
      } else if (e.button == 2) {
        console.log("点击右键");
      }
    },
    handleMouseUp(e) {
      if (e.button == 0) {
        console.log("左键释放");
      } else if (e.button == 1) {
        console.log("滚轮释放");
      } else if (e.button == 2) {
        console.log("右键释放");
        console.log("this.mouse2Down = " + this.mouse2Down);
        this.mouse2Down = false;
      }
    },
    handleMouseown(e) {
      if (e.button == 0) {
        console.log("左键按下");

      } else if (e.button == 1) {
        console.log("滚轮按下");
      } else if (e.button == 2) {
        console.log("右键按下");
        console.log("this.mouse2Down = " + this.mouse2Down);
        this.mouse2Down = true;

      }
    },
    array2DHas2D(a2D, a1D) {
      for (var el of a2D) {
        if (el.length === a1D.length) {
          for (var index in el) {
            if (el[index] !== a1D[index]) {
              break;
            }
            if (index == (el.length - 1)) {
              return true;
            }
          }
        }
      }
    },
    handleMouseOver(e) {
      var el = e.target;
      console.log("e.button = " + e.button);
      // console.log("el.dataset = " + el.dataset);
      // console.log("el.dataset.row = " + el.dataset.row);
      // console.log("el.dataset.column = " + el.dataset.column);

      if (e.button == 0) {
        console.log("\n左键覆盖移动");
        console.log("this.mouse2Down = " + this.mouse2Down);
        console.log("this.addCellD = " + this.addCellD);
        console.log("this.addCellD[this.group] = " + this.addCellD[this.group]);

        if (this.mouse2Down && this.group != '') {
          console.log("进入");
          var addL = [el.dataset.row, el.dataset.column]
          console.log("addL = " + addL);

          if (this.group in this.addCellD) {
            console.log("key has");
            if (!this.array2DHas2D(this.addCellD[this.group], addL)) {
              console.log("new + " + addL);
              this.addCellD[this.group].push(addL)
            }
          } else {
            console.log("添加");
            this.addCellD[this.group] = []
          }


        }

      } else if (e.button == 1) {
        console.log("滚轮覆盖移动");
      } else if (e.button == 2) {
        console.log("右键覆盖移动");

      }
    },



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
  background-color: #c3ffbc00;
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
  margin-top: 100px;
}

.imgBoxPixel {
  position: absolute;
  margin: auto 0;
  background: green;
  opacity: 0.7;
  z-index: 1;
  margin-top: 100px;
}

.bigHr {
  border: 3px solid rgb(129, 129, 192);
}

hr {
  border: 1px solid rgba(129, 129, 192, 0.888);
}

/* ================================= */

.slider-block {
  max-width: 1600px;
  display: flex;
  align-items: center;
}


.slider-input-number {
  width: 130px;
}

.el-slider-zoom {
  margin-top: 0;
  margin-left: 20px;
  width: 300px;
}

.el-slider-zoom-grid {
  margin-top: 0;
  margin-left: 20px;
  width: 350px;
}

.el-slider-zoom-grid-pix {
  margin-top: 0;
  margin-left: 20px;
  width: 500px;
}

.self_slider {
  margin-top: 0;
  margin-left: 20px;
  margin-right: 20px;
  width: 500px;
}

.gridsBox {
  position: absolute;
  width: 100%;
  height: 500px;
  /* background: rgba(0, 255, 98, 0.826); */
  background: rgba(0, 255, 98, 0.222);
  display: flex;
  flex-direction: row;
}

.gridCell {
  position: relative;
  /* flex-direction: column; */
  /* width: 5px;
  height: 5px; */
  /* background: rgb(255, 0, 0); */
}

.gridCell:hover {
  background: rgb(1, 255, 213);
}

/* ================================= */
</style>
