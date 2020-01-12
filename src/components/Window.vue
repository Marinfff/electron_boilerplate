<script>

  export default {
    name: 'Window',
    data() {
      return {
        name: 'puişor'
      }
    },
    mounted() {
    },
    methods: {
      send () {
        console.log('Privet')
      }
    }
  };
</script>

<template>
  <div>
    <div>Privet {{name}}!</div>
    <v-text-field
      class="mt-3"
      v-model="name"
      label="Outlined"
      placeholder="Placeholder"
      outlined
    />
    <v-btn text small color="error">Error</v-btn>
  </div>

</template>

<style scoped>

</style>
