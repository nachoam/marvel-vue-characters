<template>
    <!-- Content -->
    <div class="main-content" v-cloak>
        <!-- Left Side Filters -->
        <aside class="aside-left-filters">
          <!-- Searchbox -->
          <div class="nice-input-wrapper">
              <input v-model="searhText" type="text" name="name" placeholder="Search by name" class="nice-input" />
              <span class="focus-border"><i></i></span>
          </div>
          <div class="checkboxes-list">
              <!-- Type Selector -->
              <div class="md-checkbox">
                <input id="checkbox-grass" type="checkbox" value="captain" v-model="captainTeam">
                <label for="checkbox-grass">
                    <img src="/captain.ico" />
                    <span class="label-title">Captain Team</span>
                </label>
              </div>
              <!-- Type Selector -->
              <div class="md-checkbox">
                <input id="checkbox-poison" type="checkbox" value="ironman" v-model="ironmanTeam">
                <label for="checkbox-poison">
                    <img src="/ironman.ico" />
                    <span class="label-title">Iron-Man Team</span>
                </label>
              </div>
          </div>
        </aside>
        <!-- Characters View -->
        <main class="main-view">
          <article class="characters-list">
              <ul>
                <card :character="character" v-for="character in filterCharacters" :key="character.name" 
                  @remove_character="removeCharacter" :delete_button="true" />
              </ul>
          </article>
        </main>
    </div>
</template>

<script>
import card from '../components/card.vue';
export default {
  components: { card },
  data: () => ({
    title: 'Marvel Vue - Characters',
    api_url: '/api/nachoam/marvel-vue-characters/main/json/characters.json',
    characters:[],
    searhText:'',
    total:0,
    ironmanTeam: true,
    ironmanTeamCharactersIds:[
      1017304,1011130,1009189,1017305,1009697,1009368
    ],
    captainTeam: true,
    captainTeamCharactersIds:[
      1017327,1017311,1009297,1009562,1010740,1010801
    ]
  }),
  layout: 'civilwar',
  fetchOnServer:false,
  async fetch() {
      //Si no se almacenan por defecto los valores al borrar hay que recargar
      await this.loadData();
  },
  computed:{
    filterCharacters: function(){
      let filterCharacters = this.characters
                .filter( character => character.name.toUpperCase().includes(this.searhText.toUpperCase()))
      if(this.ironmanTeam === false){
          filterCharacters = filterCharacters.filter( character => !this.ironmanTeamCharactersIds.includes(character.id))
      }
      if(this.captainTeam === false){
          filterCharacters = filterCharacters.filter( character => !this.captainTeamCharactersIds.includes(character.id))
      }
      return filterCharacters;
    }
  },
  methods:{
    loadData: async function(){
          console.log('GO')
          let {data:response_data} = await this.$axios.get(this.api_url)
          console.log("ENTRIES:" , response_data)
          this.characters = response_data.data.results;
          this.total = response_data.data.total
    },
    removeCharacter:function(characterToRemove){
        this.characters = this.characters
                .filter( character => character !== characterToRemove)
    }
  }
}
</script>

<style></style>