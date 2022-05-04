<script setup>
import { ref, reactive } from "vue";

var query = ref("1");

const state = reactive({
  villager:{
    id:{},
    name:{},
    personality:{},
    birthday:{},
    species:{},
    hobby:{},
    catchphrase:{},
    icon:{},
    image:{},
  },
})

var requestOptions = {
  method: "GET",
  redirect: "follow",
};


fetchVillagers()
function fetchVillagers() {
  if(query.value != null && query.value != "" && query.value != undefined){
fetch(`https://acnhapi.com/v1/villagers/${query.value}`, requestOptions)
.then((response) => {
return response.json();
})
.then(setResults)
.catch((error) => console.log("error", error));
query.value= "";
  }
}

function setResults(results) {
state.villager = results;
state.villager.name = state.villager.name["name-USen"];
state.villager.icon = "https://acnhapi.com/v1/icons/villagers/" + state.villager.id;
state.villager.image = "https://acnhapi.com/v1/images/villagers/" + state.villager.id;

}

</script>

<template>
  <div>
    <div>
       <header>{{state.villager.name}}<img class=icon :src="state.villager.icon" /></header>
       <div><img :src="state.villager.image" /></div>
    </div>
    <div class=column> 
    <input
     v-model="query"
     placeholder="Enter Villager ID"
     @keyup.enter="fetchVillagers"
  />
    <h2>Personality: {{state.villager.personality}}</h2>
    <h2>Hobby: {{state.villager.hobby}}</h2>
    <h2>Species: {{state.villager.species}}</h2>
    <h2>Birthday: {{state.villager.birthday}}</h2>
  </div>
  </div>

</template>



<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  min-width: 300px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
  background-image: url(https://animalcrossingworld.com/wp-content/uploads/2020/03/my-nintendo-wallpaper-animal-crossing-new-horizons-island-landscape.jpg);
}
img {
  width:30%;
  height:50%;
  display: block;
  margin: auto;

}
.icon {
  display: inline;
  width: 5%;
  height: 5%;
}
header {
  font-size:20mm;
  font-weight: bold;
  text-align: center;
  color: black;
}
input {
  padding: 10px 10px 10px 10px;
  font-size: 1rem;
  background-color:aquamarine;
  color: rgb(0, 0, 0);
  margin: 3mm;
  
}
.column {
  text-align: center;
}
h2{
  color: black;
}
</style>