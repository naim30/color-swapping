<template>
  <div class="UploadSVG">
    <input type="file" class="file" @change="$emit('uploadFile', $event)" />
    <button class="convert" @click="$emit('convertColor')">Convert</button>
    <button class="reset" @click="$emit('resetSVG')">Reset</button>
  </div>
</template>

<script>
export default {
  name: "UploadSVG",
  emits: ["uploadFile", "resetSVG", "convertColor"],
};
</script>

<style scoped>
.UploadSVG {
  border-top: 2px solid rgb(236, 235, 235);
  height: 12%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px;
}
.file {
  width: 25%;
  position: relative;
  height: 100%;
}
.file::-webkit-file-upload-button {
  opacity: 0;
}
.file::before {
  content: "Upload SVG";
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: 600;
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: rgb(100, 100, 226);
  border-radius: 5px;
}
.file:hover::before {
  background-color: rgb(91, 91, 180);
}
.convert {
  outline: none;
  border: none;
  width: 20%;
  background-color: rgb(100, 100, 226);
  border-radius: 5px;
  height: 100%;
  color: white;
  font-weight: 600;
  font-size: 12px;
  letter-spacing: 1px;
}
.convert:hover {
  background-color: rgb(91, 91, 180);
}
.reset {
  outline: none;
  border: none;
  width: 20%;
  background-color: transparent;
  height: 100%;
  color: rgb(100, 100, 226);
  font-weight: 600;
  font-size: 15px;
  letter-spacing: 1px;
}
.reset:hover {
  color: rgb(23, 23, 207);
}
</style>
