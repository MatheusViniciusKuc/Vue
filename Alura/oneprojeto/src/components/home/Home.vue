<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
      <h1 class="centralizado"> {{ titulo }} </h1>
      <input 
        type="search" 
        class="filtro" 
        placeholder="filtre por parte do título" 
        @input="filtro = $event.target.value"
      >
      <ul class="lista-fotos">
        <li class="lista-fotos-item" v-for="foto in fotosComFiltro" :key="foto.url">
          <meu-painel :titulo="foto.titulo">
            <imagem-responsiva
              :url="foto.url"
              :titulo="foto.titulo"
              v-meu-transform.animate="15"
            />
            <meu-botao 
              rotulo="REMOVER" 
              tipo="button" 
              @botaoAtivado="remove(foto)" 
              :confirmacao="true"
              estilo="perigo"
            />
          </meu-painel>
        </li>
      </ul>
    </div>
</template>

<script>

import Painel from '../shared/painel/Painel.vue'
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue'
  
export default {
  
    components: {
      'meu-painel' : Painel,
      'imagem-responsiva': ImagemResponsiva,
      'meu-botao': Botao
    },
  
    computed: {
  
      fotosComFiltro() {
        if (this.filtro) {
          let exp = new RegExp(this.filtro.trim(), 'i')
          return this.fotos.filter(foto => exp.test(foto.titulo))
        }
        return this.fotos
      }
  
    },
  
    data () {
  
      return {
        titulo: 'Primeiro Projeto Vue',
        fotos: [],
        filtro: ''
      }
    },

    methods: {

      remove(foto) {
        alert('Remove a foto ' + foto.titulo)  
      }
    },
  
    async created() {
      try {
        const res = await this.$http.get('http://localhost:3000/v1/fotos')
        this.fotos = res.data
      } catch (err) {
        console.log(err)
      }
    }
}
</script>
  
<style>
  
    .centralizado {
      text-align: center;
    }
  
    .lista-fotos {
      list-style: none;
    }
  
    .lista-fotos .lista-fotos-item {
      display: inline-block;
    }
  
    .filtro {
      display: block;
      width: 100%;
    }
</style>
  