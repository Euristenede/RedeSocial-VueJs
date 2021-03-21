<template>
    <span>
        <div v-if="img && img != '#'" class="card-image">
          <a v-if="link && link != '#'" :href="link" target="_blank">
            <img :src="img" />
            <span class="card-title">{{titulo || ''}}</span>
          </a>
          <span v-if="!link || link == '#'">
            <img :src="img" />
            <span class="card-title">{{titulo || ''}}</span>
          </span>
        </div>

        <div class="card-content">
          <span v-if="!img || img == '#'">
            <span class="card-title">{{titulo || ''}}</span>
          </span>
          <p>{{txt}}</p>
          <p v-if="link && link != '#'"><a :href="link" target="_blank">Saiba mais...</a></p>
        </div>
    </span>
</template>

<script>
export default {
  name: "CardDetalhoVue",
  props: ['img', 'titulo', 'txt', 'link'],
  data() {
    return {};
  }
};
</script>

<!-- Add "scoped" os estilos que estiver aqui, só pertence a esse componente -->
<style scoped>
</style>
