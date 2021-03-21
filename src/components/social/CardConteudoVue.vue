<template>
  <div class="row">
    <div class="card">
      <div class="card-content">
        <span class="card-title"></span>
        <div class="row valign-wrapper">
          <grid-vue tamanho="1">
            <router-link :to="'/pagina/'+ usuarioId+'/'+$slug(nome,{lower: true})">
              <img :src="perfil" :alt="nome" class="circle responsive-img"/>
            </router-link>
            <!-- notice the "circle" class -->
          </grid-vue>
          <grid-vue tamanho="11">
            <span class="black-text">
              <strong><router-link :to="'/pagina/'+ usuarioId+'/'+$slug(nome,{lower: true})">{{nome}}</router-link></strong> - <small>{{data}}</small>
            </span>
          </grid-vue>
        </div>

        <slot></slot>
        
      </div>
      <div class="card-action">
        <p>
          <a style="cursor:pointer" @click="curtida(id)"><i class="material-icons">{{curtiu}}</i>{{this.totalCurtidas}}</a>
          <a style="cursor:pointer" @click="abreComentarios()"><i class="material-icons">insert_comment</i>{{listaComentarios.length}}</a>
        </p>
        <p v-if="exibirComentario" class="right-align">
          <input type="text" v-model="textoComentario" placeholder="Comentar">
          <button v-if="textoComentario" @click="comentar(id)" class="btn waves-effect waves-light orange"><i class="material-icons">send</i></button>
        </p>
        <p v-if="exibirComentario">
          <ul class="collection">
            <li class="collection-item avatar" v-for="item in comentarios" :key="item.id">
              <img :src="item.user.imagem" alt="" class="circle">
              <span class="title">{{item.user.name}} <small> - {{item.data}}</small></span>
              <p>
                {{item.texto}}
              </p>
            </li>
          </ul>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import GridVue from "@/components/layouts/GridVue";
export default {
  name: "CardConteudoVue",
  props: ['id', 'totalcurtidas', 'comentarios', 'curtiuconteudo','perfil', 'nome', 'data', 'usuarioId'],
  data() {
    return {
      curtiu: this.curtiuconteudo ? 'favorite' : 'favorite_border',
      totalCurtidas:this.totalcurtidas,
      exibirComentario:false,
      textoComentario:'',
      listaComentarios:this.comentarios || []
    };
  },
  components: {
    GridVue,
  },
  methods:{
    curtida(id){
      let url = '';
      if(this.$route.name == "Home"){
        url = 'conteudo/curtir/';
      }else{
        url = 'conteudo/curtirpagina/';
      }
      this.$http.put(this.$urlAPI+url+id,{},
      {"headers":{"authorization":"Bearer "+this.$store.getters.getToken}})
      .then(response => {
        if(response.data.status){
          this.totalCurtidas = response.data.curtidas;
          this.$store.commit('setConteudosLinhaTempo', response.data.lista.conteudos.data);
          if(this.curtiu == 'favorite_border'){
            this.curtiu = 'favorite';
          }else{
            this.curtiu = 'favorite_border';
          }
        }else{
          alert(response.data.erro);
        }
      })
      .catch(e => {
        console.log(e)
        alert("Erro! Tente novamente mais tarde!");
      })
    },
    abreComentarios(){
      this.exibirComentario = !this.exibirComentario;
    },
    comentar(id){
      if(!this.textoComentario){
        return;
      }
      let url = '';
      if(this.$route.name == "Home"){
        url = 'conteudo/comentar/';
      }else{
        url = 'conteudo/comentarpagina/';
      }
      this.$http.put(this.$urlAPI+url+id,{texto:this.textoComentario},
      {"headers":{"authorization":"Bearer "+this.$store.getters.getToken}})
      .then(response => {
        if(response.data){
          this.textoComentario = "";
          this.$store.commit('setConteudosLinhaTempo', response.data.lista.conteudos.data);
        }else{
          alert(response.data.erro);
        }
      })
      .catch(e => {
        console.log(e)
        alert("Erro! Tente novamente mais tarde!");
      })
    }
  }
};
</script>

<!-- Add "scoped" os estilos que estiver aqui, só pertence a esse componente -->
<style scoped>
</style>
