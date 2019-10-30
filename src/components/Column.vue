<template>
  <div :class="['column', size, offset]">
    <slot />
  </div>
</template>
<script>
export default {
  name: "b-col",
  props: {
    size: { type: String, required: true },
    offset: { type: String }
  }
};
</script>