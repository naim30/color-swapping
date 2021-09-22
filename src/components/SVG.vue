<template>
  <div class="SVG">
    <SVGcontainer :updatedSVG="updatedSVG" />
    <UploadSVG
      @uploadFile="$emit('uploadFile', $event)"
      @resetSVG="$emit('resetSVG')"
    />
  </div>
</template>

<script>
import UploadSVG from "./UploadSVG";
import SVGcontainer from "./SVGcontainer";

export default {
  name: "SVG",
  components: {
    UploadSVG,
    SVGcontainer,
  },
  props: {
    updatedSVG: String,
  },
  emits: ["uploadFile"],
};
</script>

<style scoped>
.SVG {
  width: 70%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
