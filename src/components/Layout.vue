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
    setColorsToSVG(arr) {
      for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr[i][1].length; j++) {
          this.updatedSVG =
            this.updatedSVG.substring(0, arr[i][1][j]) +
            arr[i][0] +
            this.updatedSVG.substring(arr[i][1][j] + 7);
        }
      }
    },
    hexToHSL(H) {
      let r = 0,
        g = 0,
        b = 0;
      if (H.length == 4) {
        r = "0x" + H[1] + H[1];
        g = "0x" + H[2] + H[2];
        b = "0x" + H[3] + H[3];
      } else if (H.length == 7) {
        r = "0x" + H[1] + H[2];
        g = "0x" + H[3] + H[4];
        b = "0x" + H[5] + H[6];
      }
      r /= 255;
      g /= 255;
      b /= 255;
      let cmin = Math.min(r, g, b),
        cmax = Math.max(r, g, b),
        delta = cmax - cmin,
        h = 0,
        s = 0,
        l = 0;

      if (delta == 0) h = 0;
      else if (cmax == r) h = ((g - b) / delta) % 6;
      else if (cmax == g) h = (b - r) / delta + 2;
      else h = (r - g) / delta + 4;

      h = Math.round(h * 60);

      if (h < 0) h += 360;

      l = (cmax + cmin) / 2;
      s = delta == 0 ? 0 : delta / (1 - Math.abs(2 * l - 1));
      s = +(s * 100).toFixed(1);
      l = +(l * 100).toFixed(1);

      return [h, s, l];
    },
    hslToHex(h, s, l) {
      l /= 100;
      const a = (s * Math.min(l, 1 - l)) / 100;
      const f = (n) => {
        const k = (n + h / 30) % 12;
        const color = l - a * Math.max(Math.min(k - 3, 9 - k, 1), -1);
        return Math.round(255 * color)
          .toString(16)
          .padStart(2, "0");
      };
      return `#${f(0)}${f(8)}${f(4)}`;
    },
    convertColor() {
      if (this.colors.length > 0) {
        let colors = [...this.colors];
        colors.sort(
          (a, b) => this.hexToHSL(a.color)[0] - this.hexToHSL(b.color)[0]
        );
        let updsvgcolor = this.svgColor.map((a) => [...a]);
        updsvgcolor.sort(
          (a, b) => this.hexToHSL(a[0])[0] - this.hexToHSL(b[0])[0]
        );
        let diffArr = [];
        for (let i = 0; i < colors.length - 1; i++) {
          diffArr.push(
            Math.abs(
              this.hexToHSL(colors[i].color)[0] -
                this.hexToHSL(colors[i + 1].color)[0]
            )
          );
        }
        if (diffArr.length === 0) {
          diffArr.push(this.hexToHSL(colors[0].color)[0]);
        }
        let colorArrLength = Math.abs(colors.length - updsvgcolor.length);
        let incNum = 300;
        for (let i = 0; i < colorArrLength; i++) {
          let index = diffArr.indexOf(Math.min(...diffArr));
          colors.splice(index + 1, 0, {
            id: Date.now(),
            color: colors[index].color,
          });
          diffArr[index] = incNum;
          diffArr.splice(index + 1, 0, incNum++);
        }

        for (let i = 0; i < colors.length; i++) {
          let [h, s, l] = this.hexToHSL(updsvgcolor[i][0]);
          updsvgcolor[i][0] = this.hslToHex(
            this.hexToHSL(colors[i].color)[0],
            this.hexToHSL(colors[i].color)[1],
            l
          );
        }
        this.setColorsToSVG(updsvgcolor);
      }
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
