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
          <grid-vue tamanho="4"> 
            <card-menu-vue>
              <slot name="menuesquerdo"/>
            </card-menu-vue>
            <card-menu-vue>
              <slot name="menuesquerdoamigos"/>
              
            </card-menu-vue>
          </grid-vue>
          <grid-vue tamanho="8">
            <slot name="principal"/>
          </grid-vue>
        </div>
      </div>
    </main>

    <footer>
      <footer-vue cor="blue lighten-1" logo="Social Network" descricao="Descrição da Rede Social." ano="2021">
        <li v-if="usuario"><router-link to="/" class="grey-text text-lighten-3">Home</router-link></li>
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
  name: 'SiteTemplate',
  data(){
    return{
      usuario: false
    }
  },
  created(){
    console.log('created()');
    let usuarioAux = this.$store.getters.getUsuario;
    if(usuarioAux){
      this.usuario = this.$store.getters.getUsuario;
    }else{
      this.$router.push('/login');
    }
  },
  methods:{
    sair(){
      this.$store.commit('setUsuario',null);
      sessionStorage.clear();
      this.usuario = false;
      this.$router.push('/login');
    }
  },
  components:{
    NavBar,
    FooterVue,
    GridVue,
    CardMenuVue
  }
}
</script>

<!--Esse é um estilo global, para todas as paginas-->
<style>

</style>
