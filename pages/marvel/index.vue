<template>
  <div>
    <!-- Header -->
    <header class="main-header">
        {{title}}
    </header>
    <!-- Content -->
    <div id="app" class="main-content" v-cloak>
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
    removeCharacter:function(characterToRemove){
        this.characters = this.characters
                .filter( character => character !== characterToRemove)
    },
    paginate: async function(offset_page){
        this.page = this.page + offset_page;
        await this.loadData()
    }
  }
}
</script>

<style >
:root{
    --principal-color:#e23636;
    --secondary-color:#000;
    --tertiary-color:#504A4A;
    --quaternary-color:#518CCA;
    --orange-color:#f78f3f;
    --radius-element:4px;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

:focus {
    outline: none;
}

body {
    font:400 16px/1.3 Roboto,Trebuchet MS,Helvetica,Arial,sans-serif;
    letter-spacing:.5px;
    display: flex;
    min-height: 100vh;
    flex-direction: column;
    margin: 0;
}


:focus {
    outline: none;
}

.material-icons {
    font-size: 20px;
}

body {
    font-family: 'Roboto', sans-serif;
    display: flex;
    min-height: 100vh;
    flex-direction: column;
    margin: 0;
    overflow: hidden;
}

header, article, nav, aside {
    padding: 1em;
}
header {position: sticky;top: 0;z-index: 1;}
article { height: 100vh; }
.main-header {
    text-transform: uppercase;
    background: #ec1d24;
    color: white;
    font-size: 35px;
    padding: 0.5em;
    font-weight: 400;
}

.main-content {
    display: flex;
    flex: 1;
}

.main-view {
    overflow-y: auto;
    width: 100%;
}


.aside-left-filters {
    flex: 0 0 250px;
    display: flex;
    flex-direction: column;

    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.14),
        0 1px 5px 0 rgba(0,0,0,0.12),
        0 3px 1px -2px rgba(0,0,0,0.2);
}

.characters-list {
    flex: 1;
    display: flex;
    flex-wrap: wrap;
    overflow-y: auto;
}


.checkboxes-list-title {
    margin-bottom: 0px;
}

.checkboxes-list {
    overflow-y: auto;
}



/* Nice input */
/* from https://codepen.io/Takumari85/pen/RaYwpJ */
.nice-input-wrapper {
    position: relative;
}
.nice-input{
    font: 15px/24px 'Muli', sans-serif;
    color: #333;
    width: 100%;
    box-sizing: border-box;
    border-radius: var(--radius-element);
    letter-spacing: 1px;
}
.nice-input{border: 1px solid #ccc; padding: 7px 14px 9px; transition: 0.4s;}
.nice-input ~ .focus-border:before,
.nice-input ~ .focus-border:after{content: ""; position: absolute; top: 0; left: 0; width: 0; height: 2px; background-color: #ec1d24; transition: 0.3s;}
.nice-input ~ .focus-border:after{top: auto; bottom: 0; left: auto; right: 0;}
.nice-input ~ .focus-border i:before,
.nice-input ~ .focus-border i:after{content: ""; position: absolute; top: 0; left: 0; width: 2px; height: 0; background-color: #ec1d24; transition: 0.4s;}
.nice-input ~ .focus-border i:after{left: auto; right: 0; top: auto; bottom: 0;}
.nice-input:focus ~ .focus-border:before,
.nice-input:focus ~ .focus-border:after{width: 100%; transition: 0.3s;}
.nice-input:focus ~ .focus-border i:before,
.nice-input:focus ~ .focus-border i:after{height: 100%; transition: 0.4s;}
/* End Nice input */

/* Nice checkbox */
/* from https://codepen.io/hansmaad/pen/qaGrQL */
.md-checkbox {
  position: relative;
  display: flex;
  align-items: center;
  margin: 16px 0;
  text-align: left;
}
.md-checkbox.md-checkbox-inline {
  display: inline-block;
}
.md-checkbox label {
  display: flex;
  align-items: center;
  cursor: pointer;
  text-transform: capitalize;
}

.md-checkbox label .label-title {
    margin: 0px 5px;
}

.md-checkbox label:before, .md-checkbox label:after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
}
.md-checkbox label:before {
  width: 20px;
  height: 20px;
  background: #fff;
  border: 2px solid rgba(0, 0, 0, 0.54);
  border-radius: 2px;
  cursor: pointer;
  transition: background 0.3s;
}
.md-checkbox input[type="checkbox"] {
  outline: 0;
  margin-right: 10px;
  visibility: hidden;
}
.md-checkbox input[type="checkbox"]:checked + label:before {
  background: var(--principal-color);
  border: none;
}
.md-checkbox input[type="checkbox"]:checked + label:after {
  transform: rotate(-45deg);
  top: 5px;
  left: 4px;
  width: 12px;
  height: 6px;
  border: 2px solid #fff;
  border-top-style: none;
  border-right-style: none;
}
.md-checkbox input[type="checkbox"]:disabled + label:before {
  border-color: rgba(0, 0, 0, 0.26);
}
.md-checkbox input[type="checkbox"]:disabled:checked + label:before {
  background: rgba(0, 0, 0, 0.26);
}
/* End Nice checkbox */

/* Card characters */

ul {
    display: flex;
    flex-flow: wrap;
    list-style: none;
    margin-bottom: 100px;

}

.card-content {
    position: relative;
    transform-style: preserve-3d;
    text-align: center;
}

.card-content span {
    position: absolute;
    width: 100%;
    color: white;
    text-shadow: 
    -1px -1px 1px #111, 
    1px 1px 1px #363636;

    transition: 400ms ease-in-out opacity;
}

.card-content .title {
    opacity: 0;
    font-size: 30px;
    transform: translateY(100px) translateZ(0px);
    transition: 400ms ease-in-out all;
}

.card-content .description {
    opacity: 0;
    transform: translateY(150px) translateZ(0px);
    transition: 400ms ease-in-out all;
}

.card-content img { 
    z-index: -1;
    border-radius: 20px;
    transform: scale(0.9, 0.9);
    width: 250pt;
    transition: 300ms ease-in-out all;
}

.card-content:hover img {
    -webkit-box-shadow: 0px 10px 74px -3px rgba(0,0,0,1);
    -moz-box-shadow: 0px 10px 74px -3px rgba(0,0,0,1);
    box-shadow: 0px 10px 74px -3px rgba(0,0,0,1);
    transform: scale(1,1);
}

.card {
    box-sizing: border-box;
    margin: 3px;
    transform-style: preserve-3d;
    perspective: 1000px;
}

.shine {
    z-index: 1;
    box-sizing: border-box;
    border-radius: 20px;
    width: 100%;
    height: 100%;
    display: block;
    opacity: 0;
    background: linear-gradient(135deg, rgba(255,255,255,.25) 0%,rgba(255,255,255,0) 60%);
    transition: 300ms ease-in-out opacity;
}

/* Pagination class */

.pagination{
    padding: 1em;
    width: 100%;

    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.14),
        0 1px 5px 0 rgba(0,0,0,0.12),
        0 3px 1px -2px rgba(0,0,0,0.2);
}


.pagination, .pagination .info {
    display: inline-block;

}
button {
    background-color: var(--quaternary-color); /* Green */
    border: none;
    color: white;
    padding: 2px 4px;
    border-radius: var(--radius-element);
    text-align: center;
    cursor: pointer;

}
.pagination button{
    float: left;
    padding: 12px 32px;
    border-radius: var(--radius-element);
    text-align: center;
    font-size: 15px;
    transition-duration: 0.4s;
}

.pagination button:hover{
    box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);
}

.pagination button:disabled{
        opacity: 0.6;
        cursor: not-allowed;
      
}

.pagination .info_page {
    color: black;
    float: left;
    margin: 0px 10px;
    background-color: var(--quaternary-color); /* Green */
    border-radius: var(--radius-element);
    text-align: center;
    font-size: 15px;
    border: none;
    color: white;
    padding: 12px 12px;
}

.pagination .info_total{
    font-weight: bolder;
    font-size: 15px;
    padding: 12px 12px;
    float: right;
}

/* Loader */



.loader {
    border: 16px solid #b6afaf; /* Light grey */
    border-radius: 50%;
    width: 120px;
    height: 120px;
    animation: spin 2s linear infinite;

    border-top: 16px solid var(--principal-color);
    border-right: 16px solid var(--tertiary-color);;
    border-bottom: 16px solid var(--principal-color);
    border-left: 16px solid var(--tertiary-color);

  }
  #overlay {
    position: fixed; /* Sit on top of the page content */
    /*display: none; /* Hidden by default */
    width: 100%; /* Full width (cover the whole page) */
    height: 100%; /* Full height (cover the whole page) */
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0,0,0,0.5); /* Black background with opacity */
    z-index: 2; /* Specify a stack order in case you're using a different order for other elements */
  }
  .loader{
    background-color: var(--secondary-color);
    position: absolute;
    left: 50%;
    top: 50%;
    z-index: 1;
    width: 120px;
    height: 120px;
    margin: -76px 0 0 -76px;
    border: 16px solid #f3f3f3;
    border-radius: 50%;
    border-top: 16px solid #3498db;
    -webkit-animation: spin 2s linear infinite;
    animation: spin 2s linear infinite;
  }

  
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }


</style>
