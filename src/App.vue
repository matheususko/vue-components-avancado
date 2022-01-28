<template>
  <div id="app" class="container">
  
    <h1>Components Dinâmicos</h1>

    <button @click="componentSelecionado = 'Home'">Home</button>
    <button @click="componentSelecionado = 'PostsLista'">Posts</button>
    <button @click="componentSelecionado = 'Sobre'">Sobre</button>
    <button @click="componentSelecionado = 'Assincrono'">Assincrono</button>
    <button @click="componentSelecionado = 'Contato'">Contato</button>

    <keep-alive max="2">
      <component 
        :is="componentSelecionado"
        v-bind="propsAtuais">
      </component>
    </keep-alive>
    

  </div>
</template>

<script>

  import Home from './components/Home.vue'
  import PostsLista from './components/PostsLista.vue'
  import Sobre from './components/Sobre.vue'

  const Contato = {
    render: h => h({
      name: 'ContatoDados',
      template: '<h2>Component Anonino</h2>'
    })
  }

  export default {
    components: {
      Assincrono: () => ({
        // , reject para instanciar um erro.
        component: new Promise((resolve) => {

          setTimeout(() => {
            resolve (import('./components/Assincrono.vue'))
            // reject('Carregamento falhou!')
          }, 2000)

        }),
        loading: { template: '<p>Carregando...</p>'},
        error: { template: '<p>Erro ao carregar component</p>'},
        delay: 200,
        timeout: 3000
      }),
      Contato,
      Home,
      PostsLista,
      Sobre
    },
    data() {
      return {
        componentSelecionado: 'Home',
        posts: [
          { id: 1, titulo: 'Components no vue', conteudo: 'Components são uma das peças mais importantes no vue', autor: 'Matheus Susko'},
          { id: 2, titulo: 'Distruindo conteudo com Slots', conteudo: 'Slots podem ser usados como repositório de código HTML', autor: 'Matheus Susko'}
        ]
      }
    },
    computed: {
      propsAtuais() {
        return this.componentSelecionado === 'PostsLista' 
          ? { posts: this.posts } 
          : {}
      }
    }
  }

</script>

<style scoped>
  .container {
    width: 960px;
    margin: auto;
  }

</style>