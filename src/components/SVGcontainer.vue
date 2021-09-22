<template>
  <div class="SVGcontainer">
    <div class="svg" ref="svg" v-html="updatedSVG"></div>
  </div>
</template>

<script>
export default {
  name: "SVGcontainer",
  props: {
    updatedSVG: String,
  },
  updated() {
    this.$refs.svg.childNodes[0].style.width = 300;
    this.$refs.svg.childNodes[0].style.height = 300;
  },
};
</script>

<style scoped>
.SVGcontainer {
  height: 88%;
  padding: 10px;
}
.svg {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
