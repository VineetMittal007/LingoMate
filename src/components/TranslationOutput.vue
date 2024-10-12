<template>
  <div v-if="translation">
    <h2>Translated Text</h2>
    <p>{{ translation }}</p>
  </div>
</template>

<script>
export default {
  props: {
    translation: String,
  },
};
</script>
