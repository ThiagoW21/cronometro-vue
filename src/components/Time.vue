<script setup>
defineProps({
  time: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <h3>{{ time }}</h3>
</template>

<style scope>
h3 {
  margin: 5px;
}
</style>
