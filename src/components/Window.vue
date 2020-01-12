<script>

  export default {
    name: 'Window',
    data() {
      return {
        name: 'puişor',
        text: ''
      }
    },
    mounted() {
    },
    methods: {
      send() {
        console.log('Privet')
      }
    }
  };
</script>

<template>
  <div>
    <div>Privet {{name}}!</div>
    <div>{{text}}!</div>
    <v-text-field
      class="mt-3"
      v-model="name"
      label="Outlined"
      placeholder="Placeholder"
      outlined
    />
    <v-textarea
      v-model="text"
      solo
      name="input-7-4"
      label="Solo textarea"
    />
    <v-btn text small color="error">Error</v-btn>
  </div>

</template>

<style scoped>

</style>
