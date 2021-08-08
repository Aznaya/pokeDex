<template>
  <q-page class="flex flex-center">
    <div class="column items-center" >
      <h3>{{ name.toLocaleUpperCase() }}</h3>
      <q-img :src="url" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite o Nome do Pokémon" />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon" />
    </div>
  </q-page>
</template>

<script>
import api from "../services/api";

export default {
  name: 'PageIndex',

  data() {
    return {
      name: '',
      url: '',
      search: 'charmander'
    }
  },
  async beforeMount() {
    await this.getPokemon()
  },
  methods: {
    getPokemon() {
      api
      .get(`/pokemon/${this.search.toLowerCase()}/`)
      .then((res) => {
        this.name = res.data.name
        this.url = res.data.sprites.other.dream_world.front_default
        this.triggerPositive()
      })
      .catch ((err) => {
        this.triggerNegative()
      })
    },
    triggerPositive() {
      this.$q.notify({
        type: "positive",
        position: 'top',
        message: `Pokémon encontrado.`
      })
    },
    triggerNegative() {
      this.$q.notify({
        type: "negative",
        position: 'top',
        message: `Pokémon não encontrado. Continue procurando.`
      })

    }
  }
}
</script>
