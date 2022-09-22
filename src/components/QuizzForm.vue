<template>
  <div>Hello, {{ name }}!</div>
  <input v-model="name" />
  <button :disabled="!isNamePresent" @click="submitName">Submit</button>
</template>

<script>
export default {
  data() {
    return {
      name: "",
    };
  },
  computed: {
    isNamePresent() {
      return this.name.length > 0;
    },
  },
  methods: {
    submitName() {
      console.log(this.name);
    },
  },
};
</script>
