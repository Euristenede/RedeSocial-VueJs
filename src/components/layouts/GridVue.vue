<template>
    <div :class="'col s'+tamanho">
        <slot></slot>
    </div>
</template>

<script>
export default {
  name: 'GridVue',
  props:['tamanho'],
  data () {
    return {
      
    }
  }
}
</script>

<!-- Add "scoped" os estilos que estiver aqui, só pertence a esse componente -->
<style scoped>

</style>
