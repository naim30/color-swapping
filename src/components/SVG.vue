<template>
  <div class="SVG">
    <SVGcontainer :updatedSVG="updatedSVG" />
    <UploadSVG
      @uploadFile="$emit('uploadFile', $event)"
      @resetSVG="$emit('resetSVG')"
      @convertColor="$emit('convertColor')"
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
  emits: ["uploadFile", "convertColor"],
};
</script>

<style scoped>
.SVG {
  width: 67%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
