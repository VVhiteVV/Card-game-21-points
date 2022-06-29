<template>
<div :class="{'menu' : visibleMenu, 'menuNotActive': !visibleMenu}">
  <div class="menu__inner">
      <h2 class="menu__title" >{{message}}</h2>
    <div>Ваш счет: {{pScore}}</div>
    <div>Счет Оппонента: {{oScrore}}</div>
    <button @click="restart">Играть заного</button>
  </div>
</div>
</template>

<script>
import './menu.css'
export default {
  name: "menuGame",
  props:['message','visibleMenu','oScrore','pScore'],
  methods: {
    restart(){
      location.reload()
    }
  }
}
</script>

<style scoped>

</style>