<template>
  <div class="container">
<h2 class="text-center">¿QUERÉS PROBAR GRATIS TU SUERTE EN LAS TRAGAMONEDAS?</h2>
<p class="tag-line">Acá tenés una banda. Te sugerimos usar el selector de acá abajo para chusmearlos más fácil</p>
  <div class="selector">
   <span class="text-white">Filtrá por la inicial del nombre: </span> <select class="game-search" v-model.lazy="selected">
       <option value="selected" selected="selected">0 al 9</option>
       <option class="list-item" value="AG">A a la G</option>
       <option class="list-item" value="HM">H a la M</option>
       <option class="list-item" value="NQ">N a la Q</option>
       <option class="list-item" value="RZ">R a la Z</option>
   </select>
  </div>
  <div class="games">
    <template v-for="item in numberItems" :key="item.Name">
      <div class="game" v-if="selected === 'selected'">
        <a :href="`/juegos-gratis/juego?${item.Slug}`">
          <img :src="`/wp-content/uploads/vue/juegos/assets/images/${item.img}`" :alt="item.Name" loading="lazy" decoding="async" width="100%" height="125" format="auto">
        <p class="text-white"><b>{{item.Name}}</b></p>
      </a>
    </div>
    </template>
    <template v-for="item in agItems" :key="item.Name">
    <div class="game" v-if="selected === 'AG'">
        <a :href="`/juegos-gratis/juego?${item.Slug}`">
          <img :src="item.img" :alt="item.Name" loading="lazy" decoding="async" width="100%" height="125" format="auto">
        <p class="text-white"><b>{{item.Name}}</b></p>
      </a>
      </div>
    </template>
    <template v-for="item in hmItems" :key="item.Name">
    <div class="game" v-if="selected === 'HM'">
        <a :href="`/juegos-gratis/juego?${item.Slug}`">
          <img :src="item.img" :alt="item.Name" loading="lazy" decoding="async" width="100%" height="125" format="auto">
        <p class="text-white"><b>{{item.Name}}</b></p>
      </a>
      </div>
    </template>
    <template v-for="item in nqItems" :key="item.Name">
    <div class="game" v-if="selected === 'NQ'">
        <a :href="`/juegos-gratis/juego?${item.Slug}`">
          <img :src="item.img" :alt="item.Name" loading="lazy" decoding="async" width="100%" height="125" format="auto">
        <p class="text-white"><b>{{item.Name}}</b></p>
      </a>
      </div>
    </template>
    <template v-for="item in rzItems" :key="item.Name">
    <div  class="game" v-if="selected === 'RZ'">
        <a :href="`/juegos-gratis/juego?${item.Slug}`">
          <img :src="item.img" :alt="item.Name" loading="lazy" decoding="async" width="100%" height="125" format="auto">
        <p class="text-white"><b>{{item.Name}}</b></p>
      </a>
      </div>
    </template>
  </div>
</div>
</template>
<script>
  import { computed, ref, onMounted } from 'vue'
  
  const baserowToken = import.meta.env.VITE_API_TOKEN
  const baserowUrl = import.meta.env.VITE_API_URL
  

const freeGames = () => {  //This is the full list of free games
const items = ref([]) 
const noOpRel = 'nofollow noindex noopener'
const blankTarget = '_blank'
const selected = ref('selected')

onMounted(async () => { 

    try {  

      //const response = await fetch('/wp-content/uploads/vue/juegos/assets/apostala_spanish_ARS_slug.json') <--Static data, just in case
      const response = await fetch(baserowUrl,
          {headers: {Authorization: 'Token ' + baserowToken}})
      const res = await response.json()
      let nextPage = res.next
      items.value = res.results
      let count = 1
      while (nextPage) {
        count++
        let fetchUrl = baserowUrl+'&page=' + count.toString()
        let nresponse = await fetch(fetchUrl,{headers: {Authorization: 'Token ' + baserowToken}})
        let next_ = await nresponse.json()
        items.value = [...items.value, ...next_.results]
        console.log('Results are  '+ items.value.length);
        nextPage = next_.next
      }
      console.log(items.value)
    } catch (error) {
      console.log(error)
    }
})

const numberItems = computed(()=> items.value.filter((item) => (item.Name[0] < 'A')))

const agItems = computed(()=> items.value.filter((item) => (item.Name[0] >= 'A') && (item.Name[0] <= 'G')))

const hmItems = computed(()=> items.value.filter((item) => (item.Name[0] >= 'H') && (item.Name[0] <= 'M')))

const nqItems = computed(()=> items.value.filter((item) => (item.Name[0] >= 'N') && (item.Name[0] <= 'Q')))

const rzItems = computed(()=> items.value.filter((item) => (item.Name[0] >= 'R') && (item.Name[0] <= 'Z')))

return { items, noOpRel, blankTarget, selected, numberItems, agItems, hmItems, nqItems, rzItems }
}

export default {
  setup() {
  const games = freeGames()
  return { ...games }
}
}
</script>
<style scoped>
</style>
