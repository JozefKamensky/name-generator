<template>
  <div class="container" v-if="editing">
    <span class="letter letter-view">{{ letter }}</span>
    <input class="frequency frequency--edit" type="number" v-model="input" placeholder="Frequency" />
    <button class="button" @click="save">Save</button>
  </div>
  <div class="container" v-else>
    <span class="letter letter-edit">{{ letter }}</span>
    <span class="frequency frequency--view">{{ frequency }}</span>
    <button class="button" @click="toEdit">Edit</button>
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
      this.$emit('letterChanged', { letter: this.letter, frequency: this.input })
    }
  },
}
</script>

<style>
.container {
  padding: 5px;
  display: flex;
  flex-flow: row nowrap;
}
.letter {
  margin-right: 5px;
  width: 30px;
}
.letter--view {
  margin-right: 5px;
}
.letter--edit {
  margin-right: 1px;
}
.frequency {
  width: 50px;
}
.frequency--view {
  margin-right: 10px;
}
.frequency--edit {
  margin-right: 2px;
}
.button {
  width: 60px;
}
</style>