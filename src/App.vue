<template>
  <div id="app">
   <!-- <a href="https://zhanziyang.github.io/vue-croppa/#/file-output">D O C U M E N T A C I O N</a>
    <croppa
        v-model="myCroppa"
        :width="250"
        :height="250"
        placeholder="Seleccionar imagen"
        canvas-color="white"
        @init="onInit"
    >
      <img slot="placeholder" crossorigin="anonymous" src="https://i.ibb.co/fHS3PZX/marco.png" class="addon">
    </croppa>


    <croppa
        v-model="croppa"
        :width="250"
        :height="250"
        placeholder="Seleccionar imagen"
        placeholder-color="#000"
        :placeholder-font-size="12"
        :show-remove-button="true"
        remove-button-color="#F15B5B"
        :remove-button-size="24"
        :show-loading="true"
        canvas-color="white"
        prevent-white-space
        @init="onInit"
    >
      <img slot="placeholder" crossorigin="anonymous" src="https://i.ibb.co/fHS3PZX/marco.png" class="addon">

    </croppa>


    <croppa v-model="croppa"
            accept="image/png"
            type="file"
            :file-size-limit="102400"
            @file-type-mismatch="onFileTypeMismatch"
            @file-size-exceed="onFileSizeExceed">
    </croppa>
    <input type="button" @click="saveImage('image/jpeg')" value="Guardar">
    <input type="button" @click="saveImage2('image/png')" value="Guardar foto 2"> -->

  <!--  <h2>BASE 64 DATA</h2>

    <croppa v-model="croppa"
            initial-image="https://zhanziyang.github.io/vue-croppa/static/500.jpeg">
    </croppa>
    <div class="btns">
      <button @click="dataUrl = croppa.generateDataUrl()">Output</button>
      <button @click="dataUrl = croppa.generateDataUrl('image/jpeg')">Output JPEG</button>
      <button @click="dataUrl = croppa.generateDataUrl('image/jpeg', 0.8)">Output 20% compressed JPEG</button>
    </div>
    <br>
    <div class="size">Output size: {{dataUrl.length}}</div>
    <img :src="dataUrl"> -->

 <!--   <h2>BLOB</h2>

    <croppa v-model="croppa"
            initial-image="https://zhanziyang.github.io/vue-croppa/static/500.jpeg">
    </croppa>
    <div class="btns">
      <button @click="download">download</button>
      <button @click="download('image/jpeg')">download JPEG</button>
      <button @click="download('image/jpeg', 0.8)">download 20% compressed JPEG</button>
    </div>

    <croppa v-model="myCroppa"></croppa> -->

    <croppa v-model="croppa">
      <img slot="placeholder" crossorigin="anonymous" src="https://i.ibb.co/fHS3PZX/marco.png" class="addon">
    </croppa>
    <button @click="upload('image/jpeg', 0.8)">UPLOAD</button>




  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      myCroppa: {},
      croppa: {},
      dataUrl: '',
      name: 'ssss',
    }
  },
  methods:{
    upload(type, compressionRate) {
      if (!this.croppa.hasImage()) {
        alert('no image to upload')
        return
      }

      this.croppa.generateBlob((blob) => {
        let url = URL.createObjectURL(blob)
        let fd = new FormData()
      //  fd.append('file', blob, 'filename.jpg')
        fd.append('file', blob, this.name +'.jpg')
      ///  fd.append('other', 'blahblahblah')
        let mas = fd.getAll('file');
        console.log(url)
        console.log(mas[0])
      //  $.ajax({
      //    url: 'http://www.xxx.com/api/upload',
       //   data: fd,
       //   type: 'POST',
       //   processData: false,
       //   contentType: false,
       //   success: function(data) {
        //    alert(data)
        //  }
      //  })
      }, type, compressionRate)
    }
  ,
    download(type, compressionRate) {
      this.croppa.generateBlob((blob) => {
        var url = URL.createObjectURL(blob)
        console.log(url)
        var a = document.createElement('a');
        a.download = 'filename';
        a.href = url;
        a.click();
        URL.revokeObjectURL(url);
      }, type, compressionRate)
    },
    saveImage(type, compressionRate) {
      this.myCroppa.generateBlob((blob) => {
        var url = URL.createObjectURL(blob);
        var a = document.createElement('a');
        a.download = 'filename';
        a.href = url;
        a.click();
        URL.revokeObjectURL(url);
      }, type, compressionRate)
    },

    saveImage2(type, compressionRate) {
      this.croppa.generateBlob((blob) => {
        var url = URL.createObjectURL(blob);
        var a = document.createElement('a');
        a.download = 'filename';
        a.href = url;
        a.click();
        URL.revokeObjectURL(url);
      }, type, compressionRate)
    },
    onInit() {
      this.croppa.addClipPlugin(function (ctx, x, y, w, h) {
        /*
         * ctx: canvas context
         * x: start point (top-left corner) x coordination
         * y: start point (top-left corner) y coordination
         * w: croppa width
         * h: croppa height
        */
        console.log(x, y, w, h)
        ctx.beginPath()
        ctx.arc(x + w / 2, y + h / 2, w / 2, 0, 2 * Math.PI, true)
        ctx.closePath()
      })
    },
    onFileTypeMismatch () {
      alert('Invalid file type. Please choose a jpeg or png file.')
    },
    onFileSizeExceed () {
      alert('File size exceeds. Please choose a file smaller than 100kb.')
    }
  }

}
</script>

<style>
.croppa-container {
  display: inline-block;
  cursor: pointer;
  position: relative;
  font-size: 0;
  -ms-flex-item-align: start;
  align-self: flex-start;
  background-color: #e6e6e6;
}
.croppa-container, .croppa-container canvas {
  transition: all .3s;
}
.croppa-container svg.icon-remove {
  position: absolute;
  background: #fff;
  border-radius: 50%;
  filter: drop-shadow(-2px 2px 2px rgba(0,0,0,.7));
  z-index: 10;
  cursor: pointer;
  border: 2px solid #fff;
  top: -15.75px;
  right: -8.75px;
}
.croppa-container:hover {
  cursor: pointer;
}
</style>
