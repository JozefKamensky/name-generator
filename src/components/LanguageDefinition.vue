<template>
    <div>
      <div class="horizontalContainer">
        <div class="verticalContainer">
          <Letter
            v-for="vowel in vowels"
            v-bind:key="vowel.letter"
            :letter="vowel.letter"
            :frequency="vowel.frequency"
            @letterChanged="editLetter($event, this.vowels)"
          />
        </div>
        <div class="verticalContainer">
          <Letter
            v-for="consonant in consonants"
            v-bind:key="consonant.letter"
            :letter="consonant.letter"
            :frequency="consonant.frequency"
            @letterChanged="editLetter($event, this.consonants)"
          />
        </div>
      </div>
      <span>Enter syllable structure as string of 'c' and 'v', e.g 'cv' represents syllable of one consonant and one vowel.</span>
      <br>
      <input v-model="syllablesInput" placeholder="Syllables" />
      <button @click="generate">Generate</button>
      <button @click="clear">Clear</button>
      <ul>
        <li v-for="name in names" v-bind:key="name">
          {{ name }}
        </li>
        </ul>
    </div>
</template>

<script>
import Letter from './Letter.vue'

const rouletteRandom = vals => {
  const total = vals.map(v => v.frequency).reduce((a, b) => a + b, 0)
  const r = Math.random() * total
  let tmp = 0
  for (let val of vals) {
    tmp += val.frequency
    if (r <= tmp) {
      return val
    }
  }
}

export default {
  name: 'LanguageDefinition',
  components: {
    Letter
  },
  data() {
    return {
      // http://pi.math.cornell.edu/~mec/2003-2004/cryptography/subs/frequencies.html
        vowels: [
          {letter: 'e', frequency: 12},
          {letter: 'a', frequency:  8},
          {letter: 'o', frequency:  8},
          {letter: 'i', frequency:  7},
          {letter: 'u', frequency:  3},
          {letter: 'y', frequency:  2},
          ],
        consonants: [
          {letter: 't', frequency: 9},
          {letter: 'n', frequency: 7},
          {letter: 's', frequency: 6},
          {letter: 'r', frequency: 6},
          {letter: 'h', frequency: 6},
          {letter: 'd', frequency: 4},
          {letter: 'l', frequency: 4},
          {letter: 'c', frequency: 3},
          {letter: 'm', frequency: 3},
          {letter: 'f', frequency: 2},
          {letter: 'w', frequency: 2},
          {letter: 'g', frequency: 2},
          {letter: 'p', frequency: 2},
          {letter: 'b', frequency: 2},
          {letter: 'v', frequency: 1},
          {letter: 'k', frequency: 0.7},
          {letter: 'x', frequency: 0.2},
          {letter: 'q', frequency: 0.1},
          {letter: 'j', frequency: 0.1},
          {letter: 'z', frequency: 0.1}
        ],
        syllablesInput: '',
        names: [],
    }
  },
  methods: {
    editLetter({ letter, frequency }, list) {
      const newList = list.slice()
      const i = newList.map(v => v.letter).indexOf(letter)
      newList[i].frequency = frequency
      newList[i].editing = false
      list = newList
    },
    generate() {
      const syllables = this.syllablesInput.split(',')
      for (let i = 0; i < 10; i++) {
        const l = Math.floor(Math.random() * 3) + 2
        let res = ''
        for (let j = 0; j < l; j++) {
          const s = syllables[Math.floor(Math.random() * syllables.length)]
          res += s.split('').map(ch => {
            if (ch === 'v') {
              return rouletteRandom(this.vowels).letter
            }
            return rouletteRandom(this.consonants).letter
          }).join('')
        }
        this.names.push(res)
      }
    },
    clear() {
      this.names = []
    }
  }
}
</script>

<style>
.horizontalContainer {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-evenly;
}
.verticalContainer {
  display: flex;
  flex-flow: column nowrap;
  justify-content: flex-start;
}
.width--400 {
  width: 400px;
}
</style>