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
          <!--div class="pagination">
            <button type="button" id="btn_prev" onclick="paginate(-1);" disabled >Prev</button> 
            <div id="page" class="info_page">1</div>
            <button type="button" id="btn_next" onclick="paginate(1);"  disabled >Next</button> 
            <div class="info_total">{{total}} CHARACTERS FOUND</div>
          </div-->
          <article class="characters-list">
              <ul>
                <li class="card" v-for="character in filterCharacters" :key="character.name">
                    <div class="card-content" style="transition: all 300ms ease-in-out 0s; transform: rotateY(0deg) rotateX(0deg);">
                        {{character.name}}<button style="" type="button" @click="removeCharacter(character)">X</button> 
                        <span class="shine" style="opacity: 0; background: rgba(0, 0, 0, 0) linear-gradient(-70.4116deg, rgba(255, 255, 255, 0.38) 0%, rgba(255, 255, 255, 0) 80%) repeat scroll 0% 0%;"></span>
                        <span class="title" >
                          {{character.name}} 
                        </span>
                        <span class="description" >
                          {{character.description}}
                        </span>
                        <img v-bind:src="`${character.thumbnail.path}.${character.thumbnail.extension}`" />
                    </div>
                </li>
              </ul>
          </article>
        </main>
    </div>
</template>

<script>
export default {
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