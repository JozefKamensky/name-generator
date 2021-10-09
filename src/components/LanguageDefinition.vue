<template>
    <div>
        <input v-model="customVowelsInput" placeholder="Custom vowels" />
        <input v-model="customConsonantsInput" placeholder="Custom consonants" />
        <input v-model="preferredInput" placeholder="Preferred letters" />
        <input v-model="ommitedInput" placeholder="Custom letters" />
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
export default {
  name: 'LanguageDefinition',
  data() {
    return {
        vowels: ["a","e","i","o","u","y"],
        consonants: ["b","c","d","f","g","h","j","k","l","m","n","p","q","r","s","t","v","w","x","z"],
        customVowelsInput: '',
        customConsonantsInput: '',
        preferredInput: '',
        ommitedInput: '',
        syllablesInput: '',
        names: [],
    }
  },
  methods: {
    generate() {
      // const customVowels = this.customVowelsInput.split(',')
      // const customConsonants = this.customConsonantsInput.split(',')
      // const preferred = this.preferredInput.split(', ')
      // const ommitted = this.ommitedLetters.split(',')
      const syllables = this.syllablesInput.split(',')
      for (let i = 0; i < 10; i++) {
        const l = Math.floor(Math.random() * 3) + 2
        let res = ''
        for (let j = 0; j < l; j++) {
          const s = syllables[Math.floor(Math.random() * syllables.length)]
          res += s.split('').map(ch => {
            if (ch === 'v') {
              return this.vowels[Math.floor(Math.random() * this.vowels.length)]
            }
            return this.consonants[Math.floor(Math.random() * this.consonants.length)]
          }).join('')
        }
        console.log(res)
        this.names.push(res)
      }
      console.log(this.names)
    }
  }
}
</script>