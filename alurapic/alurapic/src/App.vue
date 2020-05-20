
<template>
  <div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>

    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="Filtre por parte do titulo">
    {{filtro}}
    <ul class="Lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro" :key="foto.item">
        <meu-painel :titulo="foto.titulo">
          <img class="img-responsiva" :src="foto.url" :alt="foto.titulo" />
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from './components/shared/painel/Painel.vue';
export default {
  components: {
    'meu-painel' : Painel
  },
  
  data() {
    return {
      titulo: "Alurapic",
      fotos: [],
      filtro: ''
    };
  },

  computed: {
    fotosComFiltro () {
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos;
      }
    }
  },

  created() {
    this.$http
      .get("http://localhost:3000/v1/fotos")
      .then(res => res.json())
      .then(
        fotos => (this.fotos = fotos),
        err => console.log(err)
      );
  }
};
</script>

<style>
.corpo {
  font-family: Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.corpo .centralizado {
  text-align: center;
}

.lista-fotos {
  list-style: none;
}

.lista-fotos-item {
  display: inline-block;
}

.img-responsiva {
  width: 100%;
}

.filtro {
  width: 100%;
  display: block;
}
</style>
