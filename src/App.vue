<template>
  <div class="cards">

    <div 
  v-for="p in pokemons" 
  :key="p.id"
  @click="fetchEvolutions(p)"

  class="card"
  >
    
    <div class="title">
      {{p.name}} #{{p.id}}
    </div>

    <div class="content">
      <img :src="p.sprite" />
    </div>

    <div 
    v-for="type in p.types" 
    :key="type.name"
    class="description">
    {{type.name}}
      
    </div>

  </div>

  <div 
  v-for="e in evolutions" 
  :key="e.id"

  class="card"
  >
    
    <div class="title">
      {{e.name}} #{{e.id}}
    </div>

    <div class="content">
      <img :src="e.sprite" />
    </div>

    <div 
    v-for="type in e.types" 
    :key="type.name"
    class="description">
    {{type.name}}
      
    </div>

  </div>

  </div>
  
 
</template>


<script>
import { ref,reactive,onMounted} from 'vue';

export default{

setup(){

  const api='https://pokeapi.co/api/v2/pokemon'
  const ids=[1,4,7]
  const pokemons=ref(null)
  const evolutions=ref(null)
  const selectedId=ref(null)

 onMounted(async()=>{
    pokemons.value=await fetchData(ids)
  })
  async function fetchData(ids){
      const responses=await Promise.all(
        ids.map(id=> window.fetch(`${api}/${id}`))
      )
      const data=await Promise.all(
        responses.map(response=>response.json())
      )
      
      return data.map(datum=>({
        id:datum.id,
        name:datum.name,
        sprite:datum.sprites.other['official-artwork'].front_default,
        types:datum.types.map(type=>{
          return{
            name:type.type.name
          }
        })
      }))

    }

    async function fetchEvolutions(pokemon){
      evolutions.value=await fetchData(
        [pokemon.id+1, pokemon.id+2]
      )
      selectedId.value=pokemon.id
      console.log(evolutions);
      console.log(selectedId.value);
    }



  return{
    pokemons,
    fetchEvolutions,
    selectedId,
    evolutions
    
  }
}

}




</script>


<style scoped>
.card {
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3px darkgrey;
  transition: 0.2s;
}
.title, .content, .description {
  padding: 16px;
  text-transform: capitalize;
  text-align: center;
}
.title, .content {
  border-bottom: 1px solid silver;
}
.title {
  font-size: 1.25em;
}
.card:hover {
  transition: 0.2s;
  box-shadow: 0px 1px 9px darkgrey;
}

  img {
  width: 100%;
  display: block;
}
.cards {
  display: flex;

  justify-content: center;
  margin-top: 15px;
}
.opace {
  opacity: 0.5;
}
.card:hover {
  opacity: 1;
}
.evolutions{
  display: flex;
}

</style>
