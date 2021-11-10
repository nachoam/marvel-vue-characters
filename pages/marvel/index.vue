<template>
  <div class="main-content" >
      <!-- Left Side Filters -->
      <aside class="aside-left-filters">
        <!-- Searchbox -->
        <div class="nice-input-wrapper">
            <input v-model="searhText" type="text" name="name" placeholder="Search by name" class="nice-input" />
            <span class="focus-border"><i></i></span>
        </div>
      </aside>
      <!-- Characters View -->
      <main class="main-view">
        <div class="pagination">
          <button type="button"  @click="paginate(-1)" :disabled="disablePrev" >Prev </button> 
          <div  class="info_page">1</div>
          <button type="button"  @click="paginate(1)"  :disabled="disableNext" >Next</button> 
          <div class="info_total">{{total}} CHARACTERS FOUND</div>
        </div>
        <article class="characters-list">
            <ul>
              <li class="card" v-for="character in characters" :key="character.name">
                  <div class="card-content" style="transition: all 300ms ease-in-out 0s; transform: rotateY(0deg) rotateX(0deg);">
                      {{character.name}}
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
    // INSERT HERE YOUR HASH AND PUBLIC API MARVEL KEY
    // HOW TO CREATE: https://medium.com/zurvin/jugando-con-la-api-de-marvel-y-javascript-c3afad762c2c
    //apiKey : "ts=1000&apikey=<PUBLIC_API_KEY>&hash=<HASH GENERATED WITH TS+PRIVATE_API_KEY+PUBLIC_API_KEY>".
    apiKey:"ts=1000&apikey=7333dd5c243af601409025591ed2bb25&hash=9adfe05749fab0b0a79dd49da77ab153",
    characters:[],
    searhText:'',
    total:0,
    page:1,
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
  watch:{
    searhText(val){
      this.loadData()
    },
  },
  computed:{
    disableNext: function(){
      return (this.page) * 20 >= this.total;
    },
    disablePrev: function(){
      return this.page === 1;
    }
  },
  methods:{
    loadData: async function(){
          console.log('GO')
          let current_page = this.page - 1;
          let filter = this.searhText ? `nameStartsWith=${this.searhText}&` : "";
          let offset = current_page === 0 ? 0 : 20 * current_page;            
          let {data:response_data} = await this.$axios.get(`/apimarvel/v1/public/characters?${filter}offset=${offset}&${this.apiKey}`)
          console.log("ENTRIES:" , response_data)
          this.characters = response_data.data.results;
          this.total = response_data.data.total
    },
    paginate: async function(offset_page){
        this.page = this.page + offset_page;
        await this.loadData()
    }
  }
}
</script>

<style >
</style>
