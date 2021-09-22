<template>
  <div class="AddColor">
    <div class="Colors">
      <div class="empty" v-if="colors.length === 0">Select colors...</div>
      <Color
        v-else
        v-for="color of colors"
        :key="color.id"
        :color="color"
        @deleteColor="$emit('deleteColor', color.id)"
      />
    </div>
    <AddButton
      @changeColor="$emit('changeColor', $event)"
      @addColor="$emit('addColor')"
      :selectedColor="selectedColor"
    />
  </div>
</template>

<script>
import Color from "./Color.vue";
import AddButton from "./AddButton.vue";

export default {
  name: "AddColor",
  components: {
    Color,
    AddButton,
  },
  props: {
    colors: Array,
    selectedColor: String,
  },
  emits: ["changeColor", "addColor", "deleteColor"],
};
</script>

<style scoped>
.AddColor {
  height: 100%;
  width: 30%;
  padding: 5px;
  overflow-y: scroll;
}
.AddColor::-webkit-scrollbar {
  width: 2px;
}
.AddColor::-webkit-scrollbar-track {
  background: rgb(236, 235, 235);
}
.AddColor::-webkit-scrollbar-thumb {
  background: #888;
}
.AddColor::-webkit-scrollbar-thumb:hover {
  background: #555;
}
.empty {
  display: flex;
  justify-content: center;
  color: rgb(196, 196, 196);
  padding: 10px;
  font-size: 15px;
  font-weight: 500;
  letter-spacing: 1px;
}
</style>
