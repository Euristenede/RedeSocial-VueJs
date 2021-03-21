<template>
  <span>
    <header>
      <nav-bar logo="Social Network" url="/" cor="blue lighten-1">
        <li v-if="!usuario"><router-link to="/login">Entrar</router-link></li>
        <li v-if="!usuario"><router-link to="/cadastro">Cadastre-se</router-link></li>
        <li v-if="usuario"><router-link to="/perfil">{{usuario.name}}</router-link></li>
        <li v-if="usuario"><a v-on:click="sair()">Sair</a></li>
      </nav-bar>
    </header>

    <main>
      <div class="container">
        <div class="row">
          <grid-vue tamanho="8"> 
            <card-menu-vue>
              <slot name="menuesquerdo"/>
            </card-menu-vue>
          </grid-vue>
          <grid-vue tamanho="4">
            <slot name="principal"/>
          </grid-vue>
        </div>
      </div>
    </main>

    <footer>
      <footer-vue cor="blue lighten-1" logo="Social Network" descricao="Projeto Rede Social UTFPR-SH." ano="2020">
        <li><a class="grey-text text-lighten-3" href="#!">Home</a></li>
      </footer-vue>
    </footer>
    
  </span>
</template>

<script>
import NavBar from '@/components/layouts/NavBar'
import FooterVue from '@/components/layouts/FooterVue'
import GridVue from '@/components/layouts/GridVue'
import CardMenuVue from '@/components/layouts/CardMenuVue'

export default {
  data(){
    return{
      usuario: false
    }
  },
  name: 'LoginTemplate',
  components:{
    NavBar,
    FooterVue,
    GridVue,
    CardMenuVue
  },
  created(){
    console.log('created()');
    let usuarioAux = this.$store.getters.getUsuario;
    if(usuarioAux){
      this.usuario = this.$store.getters.getUsuario;
      this.$router.push('/');
    }
  }
}
</script>

<!--Esse é um estilo global, para todas as paginas-->
<style>

</style>
