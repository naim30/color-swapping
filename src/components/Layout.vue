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
      selectedColor: "#808080",
      initalSVG: null,
      updatedSVG: null,
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
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(file);
      }
    },
    resetSVG() {
      this.updatedSVG = this.initalSVG;
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
