<template>
  <div>
    <div v-for="address in data.addresses">
      <div>{{address.city.name}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['data', 'index', 'column']
}
</script>
