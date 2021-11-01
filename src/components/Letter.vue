<template>
  <div v-if="editing">
    <span>{{ letter }}</span>
    <input type="number" v-model="input" placeholder="Frequency" />
    <button @click="save">Save</button>
  </div>
  <div v-else>
    <span>{{ letter }}</span>
    <span>{{ frequency }}</span>
    <button @click="toEdit">Edit</button>
  </div>
</template>

<script>
export default {
  name: 'Letter',
  props: ['letter', 'frequency'],
    data() {
      return {
        input: this.frequency,
        editing: false,
      }
  },
  emits: ['letterChanged'],
  methods: {
    toEdit() {
      this.editing = true
    },
    save() {
      this.editing = false
      console.log('emit event')
      this.$emit('letterChanged', { letter: this.letter, frequency: this.input })
    }
  },
}
</script>

<style>
</style>