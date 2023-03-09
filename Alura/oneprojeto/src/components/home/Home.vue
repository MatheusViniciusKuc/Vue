<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
      <h1 class="centralizado"> {{ titulo }} </h1>

      <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>

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
            <router-link :to="{ name: 'altera', params: { id: foto._id } }">
              <meu-botao 
                rotulo="ALTERAR" 
                tipo="button"
              />
            </router-link>
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
import FotoService from '../../domain/foto/FotoService'
  
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
        filtro: '',
        mensagem: ''
      }
    },

    methods: {

      remove(foto) {

        this.service.apaga(foto._id)
          .then(
            () => {
              let indice = this.fotos.indexOf(foto);
              this.fotos.splice(indice, 1);
              this.mensagem = 'Foto removida com sucesso'
            }, 
            err => {
              this.mensagem = 'Não foi possível remover a foto';
              console.log(err);
            }
          ) 
      }
    },
  
    created() {
      this.service = new FotoService(this.$resource);

      this.service
        .lista()
        .then(fotos => this.fotos = fotos, err => console.log(err));
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
  