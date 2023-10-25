<script setup lang="ts">
import { ref } from "vue";
import data from "@/icons";

interface Props {
  color: string;
  action: string;
  viewBox: string;
  activeBorder?: boolean;
}
interface Icons {
  [key: string]: string;
}

const props = defineProps<Props>();
const emit = defineEmits<{ (e: "click", action: string): void }>();

const icons = ref<Icons>(data);
const activeClass = ref(false);

const click = () => {
  emit("click", props.action);

  if (props.activeBorder) {
    activeClass.value = !activeClass.value;
  }
};
</script>
<template>
  <button type="button" @click="click" :class="{ active: activeClass }">
    <svg xmlns="http://www.w3.org/2000/svg" height="1em" :viewBox="viewBox">
      <path :fill="color" :d="icons[action]" />
    </svg>
  </button>
</template>
