<template>
  <div class="Color">
    <div class="container">
      <div class="color-box" :style="{ backgroundColor: color.color }"></div>
      <div class="color-name">{{ color.color.toUpperCase() }}</div>
    </div>
    <div class="delete" @click="$emit('deleteColor', color.id)">
      <span class="material-icons">
        delete
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Color",
  props: {
    color: Object,
  },
  emits: ["deleteColor"],
};
</script>

<style scoped>
.Color {
  border: 1px solid rgb(209, 204, 204);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 2px;
  margin-bottom: 10px;
  font-size: 14px;
  border-radius: 2px;
}
.container {
  display: flex;
  align-items: center;
}
.color-box {
  height: 35px;
  width: 35px;
}
.color-name {
  padding-left: 10px;
  color: rgb(129, 129, 129);
  font-size: 12px;
  letter-spacing: 1px;
  font-weight: 600;
}
.delete {
  height: 35px;
  width: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: rgb(224, 14, 14);
  cursor: pointer;
  user-select: none;
}
.delete:hover {
  background: rgb(240, 240, 240);
}
.material-icons {
  font-family: "Material Icons";
  font-size: 18px;
  font-weight: 300;
}
</style>
