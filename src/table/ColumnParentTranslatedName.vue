<template>
  <div v-if="data.parent">
    {{ translate(data.parent).name }}
  </div>
</template>

<script>
  export default {
    props: ['data', 'index', 'column']
  }
</script>
