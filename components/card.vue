<template>
    <li class="card">
        <div class="card-content" >
            <img v-bind:src="`${character.thumbnail.path}.${character.thumbnail.extension}`" width="300px"/>
            <span class="title" > {{character.name}}</span>            
            <button v-if="delete_button" class="delete" type="button" @click="removeCharacter(character)">X</button> 
        </div>
    </li>
</template>
<script >

export default {
  props:["character","delete_button"],
  data:()=> ({}),
  methods:{
    removeCharacter(characterToRemove){
        console.log('emit remove_character event', characterToRemove)
          this.$emit('remove_character',characterToRemove)
    }
  }
}
</script>
<style scoped>

        
                

.card {
    box-sizing: border-box;
    margin: 3px;
    transform-style: preserve-3d;
    perspective: 1000px;
}


.card-content {
    position: relative;
    transform-style: preserve-3d;
    text-align: center;
}


.card-content span {
    position: absolute;
    top: 140px;
    left: 5px;
    width: 100%;
    color: white;
    text-shadow: 
    -1px -1px 1px #111, 
    1px 1px 1px #363636;

    transition: 400ms ease-in-out opacity;
}

.card-content .title {
    opacity: 1;
    font-size: 18px;
    transition: 400ms ease-in-out all;
}

.card-content:hover .title {
    font-size: 25px;
    transform: scale(1,1);
}

.card-content .delete{
    position: absolute;
    top: 25px;
    right: 25px;
    height: 22px;
    width: 22px;
    border-radius: 20px;
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



</style>
