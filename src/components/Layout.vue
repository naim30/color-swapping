<template>
  <div class="Layout">
    <AddColor
      :colors="colors"
      :selectedColor="selectedColor"
      @changeColor="changeColor"
      @addColor="addColor"
      @deleteColor="deleteColor"
    />
    <SVG
      @uploadFile="uploadFile"
      :updatedSVG="updatedSVG"
      @resetSVG="resetSVG"
      @convertColor="convertColor"
    />
  </div>
</template>

<script>
import AddColor from "./AddColor.vue";
import SVG from "./SVG.vue";

export default {
  name: "Layout",
  components: {
    AddColor,
    SVG,
  },
  data: function() {
    return {
      colors: [],
      selectedColor: "#ff0000",
      initalSVG: null,
      updatedSVG: null,
      svgColor: [],
    };
  },
  methods: {
    changeColor(event) {
      this.selectedColor = event.target.value;
    },
    addColor() {
      if (
        this.colors.findIndex((color) => color.color === this.selectedColor) ===
        -1
      ) {
        this.colors.push({
          id: Date.now(),
          color: this.selectedColor,
        });
      }
    },
    deleteColor(id) {
      let index = this.colors.findIndex((color) => color.id === id);
      this.colors.splice(index, 1);
    },
    uploadFile(event) {
      let file = event.target.files[0];
      const reader = new FileReader();
      if (file.name.includes(".svg")) {
        reader.onload = (res) => {
          this.initalSVG = res.target.result;
          this.updatedSVG = res.target.result;
          this.getSVGColor(res.target.result);
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(file);
      }
    },
    resetSVG() {
      this.updatedSVG = this.initalSVG;
    },
    indexes(source, find) {
      if (!source) {
        return [];
      }
      if (!find) {
        return source.split("").map(function(_, i) {
          return i;
        });
      }
      var result = [];
      for (let i = 0; i < source.length; ++i) {
        if (source.substring(i, i + find.length) == find) {
          result.push(i);
        }
      }
      return result;
    },
    getSVGColor(initialSVG) {
      const regx = new RegExp(/#[a-fA-F0-9]{6}|rgba?\([\d\s,.]*\)/gi);
      let svgColor = [...new Set(initialSVG.match(regx))].map((color) => [
        color,
        this.indexes(initialSVG, color),
      ]);
      this.svgColor = svgColor;
    },
    convertColor() {
      console.log(this.colors);
      console.log(this.svgColor);
    },
  },
};
</script>

<style scoped>
.Layout {
  display: flex;
  height: 400px;
  width: 700px;
  border: 2px solid rgb(236, 235, 235);
  border-radius: 3px;
}
</style>
