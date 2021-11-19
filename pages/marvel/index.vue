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
          <div  class="info_page">{{page}}</div>
          <button type="button"  @click="paginate(1)"  :disabled="disableNext" >Next</button> 
          <div class="info_total">{{total}} CHARACTERS FOUND</div>
        </div>
        <article class="characters-list">
            <ul>
              <card :character="character" v-for="character in characters" :key="character.name" 
                    :delete_button="false" />
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
    apiKey : "ts=1000&apikey=<PUBLIC_API_KEY>&hash=<HASH GENERATED WITH TS+PRIVATE_API_KEY+PUBLIC_API_KEY>".
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
    ],
    controlFilterTime:null
  }),
  fetchOnServer:false,
  async fetch() {
      //Si no se almacenan por defecto los valores al borrar hay que recargar
      await this.loadData();
  },
  watch:{
    searhText(val){
        if(this.controlFilterTime)clearTimeout(this.controlFilterTime)
        this.controlFilterTime = setTimeout(async () => {
          this.page = 1
          await this.loadData()
        }, 500);
    }
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
