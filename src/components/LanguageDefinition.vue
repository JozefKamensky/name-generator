<template>
    <div>
      <table>
        <tr><th>Vowel</th><th>Frequency</th><th>Actions</th></tr>
        <tr v-for="vowel in vowels" v-bind:key="vowel.letter">
          <!-- <td>{{ vowel.letter }}</td>
          <td v-if="!vowel.editing">{{ vowel.frequency }}</td>
          <td v-else></td>
          <td><button @click="prepareToEditVowel(vowel.letter)">Edit</button></td> -->
          <Letter :letter="vowel.letter" :frequency="vowel.frequency" @letterChanged="editVowel"/>
        </tr>
      </table>
      <input v-model="syllablesInput" placeholder="Syllables" />
      <button @click="generate">Generate</button>
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
        vowels: [
          {letter: 'e', frequency: 12, editing: false},
          {letter: 'a', frequency:  8, editing: false},
          {letter: 'o', frequency:  8, editing: false},
          {letter: 'i', frequency:  7, editing: false},
          {letter: 'u', frequency:  3, editing: false},
          {letter: 'y', frequency:  2, editing: false},
          ],
        consonants: ["b","c","d","f","g","h","j","k","l","m","n","p","q","r","s","t","v","w","x","z"],
        syllablesInput: '',
        names: [],
    }
  },
  methods: {
    editVowel({ letter, frequency }) {
      const newVowels = this.vowels.slice()
      const i = newVowels.map(v => v.letter).indexOf(letter)
      newVowels[i].frequency = frequency
      newVowels[i].editing = false
      this.vowels = newVowels
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
            return this.consonants[Math.floor(Math.random() * this.consonants.length)]
          }).join('')
        }
        this.names.push(res)
      }
    },
  }
}
</script>