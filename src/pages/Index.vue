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
    <div class="row justify-between full-width absolute container-arrows">
      <q-icon
        name="far fa-arrow-alt-circle-left"
        color="primary"
        class="q-ml-sm cursor-pointer"
        size="35px"
        @click="getPokemon(id - 1)"
      />
      <q-icon
        name="far fa-arrow-alt-circle-right"
        color="primary"
        class="q-mr-sm cursor-pointer"
        size="35px"
        @click="getPokemon(id + 1)"
      />
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
      id: null,
      search: 'charmander'
    }
  },
  async beforeMount() {
    await this.getPokemon()
  },
  methods: {
    getPokemon(id) {
      api
      .get(id > 0 ? `/pokemon/${id}/` : `/pokemon/${this.search.toLowerCase()}/`)
      .then((res) => {
        this.name = res.data.name
        this.id = res.data.id
        this.search = this.name
        this.url = res.data.sprites.other.dream_world.front_default
      })
      .catch (() => {
        this.triggerNegative()
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
