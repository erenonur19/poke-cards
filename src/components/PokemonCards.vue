<template>
<div class="cards">

<card
v-for="pokemon in pokemons"
:key="pokemon.id"
@click="click(pokemon)"
:class="{opace:selectedId && pokemon.id!==selectedId}"
class="card"
 >
 <template v-slot:title>
  {{pokemon.name}} #{{pokemon.id}}
 </template>
 <template v-slot:content>
  <img :src="pokemon.sprite"/>
 </template>
 <template v-slot:description>
 <div
 v-for="type in pokemon.types" 
 :key="type" >
 {{type.name}}

 </div>
 



 
 </template>
 
</card>

</div>
</template>

<script>
import Card from './Card.vue'
export default{
    components:{
        Card,
    },
    props:{
        selectedId:{
            type:Number
        },
        pokemons:{
            type:Array,
            default:[]
        }
    },
    setup(props,ctx){
       function click(pokemon){

        ctx.emit('chosen', pokemon)
     
        }
        return{
            click
        }
    }

}
</script>

<style scoped>

.cards {
  display: flex;

  justify-content: center;
  margin-top: 15px;
}

img {
  width: 100%;
  display: block;
}
.opace{
  opacity:0.5;
}
.card:hover{
  opacity: 1;
}
</style>