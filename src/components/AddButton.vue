<template>
  <div class="AddButton">
    <div class="container">
      <input
        type="color"
        :value="selectedColor"
        @input="$emit('changeColor', $event)"
      />
      <div class="color-name" :style="{ color: selectedColor }">
        {{ selectedColor }}
      </div>
    </div>
    <div class="add" @click="$emit('addColor')">
      <span class="material-icons">
        done
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddButton",
  props: {
    selectedColor: String,
  },
  emits: ["changeColor", "addColor"],
};
</script>

<style scoped>
.AddButton {
  background: #eeeeee;
  padding: 2px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 40px;
}
.container {
  display: flex;
  align-items: center;
}
.container > input {
  width: 35px;
  height: 35px;
  padding: 0;
  margin: 0;
  background: transparent;
  border: none;
  cursor: pointer;
  border-radius: 100%;
}
.container > input::-webkit-color-swatch-wrapper {
  padding: 0px;
  border-radius: 100%;
}
.container > input::-webkit-color-swatch {
  border: none;
  border-radius: 100%;
}
.color-name {
  padding-left: 10px;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 1px;
}
.add {
  width: 35px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  background-color: rgb(100, 100, 226);
  border-radius: 100%;
  cursor: pointer;
  user-select: none;
}
.add:hover {
  background-color: rgb(92, 24, 202);
}
.material-icons {
  font-family: "Material Icons";
  font-weight: 600;
}
</style>
