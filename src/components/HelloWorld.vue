<script>
import { setTransitionHooks } from "vue";
export default {
  data() {
    return {
      currentTool: "default",
      current_x: 0,
      current_y: 0,
      currentColor: "",
      link: "",
    };
  },
  methods: {
    LoadByFile(e) {
      var reader,
        files = e.target.files;

      var reader = new FileReader();
      var canvas = this.$refs.imageCanvas;
      var ctx = canvas.getContext("2d");

      reader.onload = (e) => {
        var img = new Image();
        img.onload = function () {
          let scale_factor = Math.min(
            canvas.width / img.width,
            canvas.height / img.height
          );

          let newWidth = img.width * scale_factor;
          let newHeight = img.height * scale_factor;

          let x = canvas.width / 2 - newWidth / 2;
          let y = canvas.height / 2 - newHeight / 2;

          ctx.drawImage(img, x, y, newWidth, newHeight);
        };
        img.src = event.target.result;
      };
      reader.readAsDataURL(files[0]);
    },

    loadByLink(e) {
      console.log(this.link);
      var img = new Image();
      img.crossOrigin = "anonymous";
      var canvas = this.$refs.imageCanvas;
      var ctx = canvas.getContext("2d");
      img.onload = function () {
        let scale_factor = Math.min(
          canvas.width / img.width,
          canvas.height / img.height
        );

        let newWidth = img.width * scale_factor;
        let newHeight = img.height * scale_factor;

        let x = canvas.width / 2 - newWidth / 2;
        let y = canvas.height / 2 - newHeight / 2;

        ctx.drawImage(img, x, y, newWidth, newHeight);
      };
      img.src = this.link;
    },

    activateTool(toolName) {
      this.currentTool = toolName;
    },

    executeTool() {
      switch (this.currentTool) {
        case "colorPicker":
          this.colorPickerAction();
          break;
      }
    },

    colorPickerAction() {
      var canvas = this.$refs.imageCanvas;

      var ctx = canvas.getContext("2d", { willReadFrequently: true });

      const pixel = ctx.getImageData(this.current_x, this.current_y, 1, 1);
      const data = pixel.data;

      const rgbColor = `rgb(${data[0]} ${data[1]} ${data[2]} / ${
        data[3] / 255
      })`;

      this.$refs.colorBox.style.backgroundColor = rgbColor;

      console.log(rgbColor);
    },

    showCoordinates(e) {
      var canvas = this.$refs.imageCanvas;
      this.current_x = Math.floor(
        e.clientX - canvas.getBoundingClientRect().left
      );
      this.current_y = Math.floor(
        e.clientY - canvas.getBoundingClientRect().top
      );
    },

    clearCanvas() {
      var canvas = this.$refs.imageCanvas;
      const context = canvas.getContext("2d");
      context.clearRect(0, 0, canvas.width, canvas.height);
    },
  },
};
</script>

<template>
  <div class="canvas_page">
    <div class="tools">
      <input type="file" id="imageLoader" @change="LoadByFile" />
      <div class="inpLink">
        <input v-model="link" ref="linkInput" type="text" id="imageLoader" />
        <button class="inp-button" v-on:click="loadByLink">Load</button>
      </div>

      <div class="tools__wrapper">
        <div v-on:click="activateTool('colorPicker')" class="tools__inner">
          <p>Color picker</p>
        </div>
        <div @click="clearCanvas" class="tools__inner">
          <p>Clear</p>
        </div>
      </div>
      <div class="info"></div>
      <div
        ref="colorBox"
        class="color-picker_color_box"
        :color="currentColor"
      ></div>
      <span>x:{{ current_x }}, y:{{ current_y }}</span>
    </div>
    <canvas
      @mousemove="showCoordinates"
      v-on:click="executeTool"
      id="imageCanvas"
      ref="imageCanvas"
      width="1000"
      height="700"
    ></canvas>
  </div>
</template>

<style scoped>
.inp-button {
  border: 1px solid black;
  background: wheat;
  color: white;
}
.inpLink {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.info {
  display: flex;
  flex-direction: row;
}
.color-picker_color_box {
  width: 20px;
  height: 20px;
  color: currentColor;
}

#imageCanvas {
  display: block;

  width: 1000px;
  height: 700px;
  margin: 20px 20px 20px 0;
  background-color: rgb(45, 45, 45);
}

#imgeLoader {
  margin-bottom: 60px;
}

.tools {
  width: 10% !important;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px 0;
}

.tools__wrapper {
  width: 90%;
  height: 60%;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.tools__inner {
  background-color: white;
  width: 100%;
  border-radius: 15px;
  padding: 5px;
  margin: 2px;
}

.canvas_page {
  height: 100%;
  display: flex;
  justify-content: space-evenly;
}
</style>
