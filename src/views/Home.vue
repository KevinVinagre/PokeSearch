<template>
  <div class="home">
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-5">
          <ProcuraPokemon
          :urlApi="urlApi"
          @definirUrlPokemon="definirUrlPokemon"/>
        </div>
      </div>
      <ListaPokemon 
      :urlImagem="urlImagem" 
      :urlApi="urlApi"
      @definirUrlPokemon="definirUrlPokemon"/>
      <DetalhePokemon
      v-if="mostraDetalhes"
      :urlPokemon="urlPokemon"
      :urlImagem="urlImagem"
      @fecharDetalhes="fecharDetalhes"/>
    </div>
  </div>
</template>

<script>

import ListaPokemon from '@/components/ListaPokemon.vue';
import DetalhePokemon from '@/components/DetalhePokemon.vue';
import ProcuraPokemon from '@/components/ProcuraPokemon.vue';

export default {
  name: 'Home',
  components: {
    ListaPokemon,
    DetalhePokemon,
    ProcuraPokemon
  },
  data: ()=> {
    return {
      urlImagem: 'https://pokeres.bastionbot.org/images/pokemon/',
      urlApi: 'https://pokeapi.co/api/v2/pokemon/',
      urlPokemon: '',
      mostraDetalhes: false
    }
  },
  methods:
  {
    definirUrlPokemon(url)
    {
      this.urlPokemon = url;
      this.mostraDetalhes = true;
    },
    fecharDetalhes()
    {
      this.urlPokemon = '';
      this.mostraDetalhes = false;
    }
  }
}
</script>