<template lang="html">
  <div>
    <h1>Characters</h1>
    <character-filter-form :characters="characters" />
    <character-detail :character="selectedCharacter"/>
  </div>
</template>

<script>
import CharacterFilterForm from './components/CharacterFilterForm.vue'
import CharacterDetail from './components/CharacterDetail.vue'
import {eventBus} from './main.js';

export default {
  data(){
    return {
      characters: [],
      selectedCharacter: null,
      urls: []
    }
  },
  components: {
    "character-filter-form": CharacterFilterForm,
    "character-detail": CharacterDetail
  },
  mounted(){
    // this.getData2()
    this.getUrls()
    this.getData()
    // Promise.all(this.urls.map(url => fetch(url)))
    // .then(responses => Promise.all(responses.map(res => res.json())))
    // .then(data =>  Promise.all(data.flatMap(chars => chars.results))
    // .then((values) => {this.new_characters.push(...values)}))


   

    eventBus.$on('character-selected', (character) => {
      this.selectedCharacter = character
    })
  },
  methods: {
      getData: async function(){
        const responses = await Promise.all(this.urls.map(url => fetch(url)))
        const data = await Promise.all(responses.map(res => res.json()))
        const values = await Promise.all(data.flatMap(chars => chars.results))
        this.characters.push(...values)
      },

      // getData: async function() {
      // const response = await fetch('https://rickandmortyapi.com/api/character/')
      // const data = await response.json()
      // this.characters = data.results
      // return data.info.pages
      getUrls: function() {
        // const pages = this.getData()
        // console.log(pages)
        for (let i=1; i<=34; i++) {
          let url = 'https://rickandmortyapi.com/api/character/?page=' + i
          this.urls.push(url)
        }
      }
  
  }
}
</script>

<style lang="css" scoped>
h1 {
  text-align: center;
  color: #333;
}
</style>
