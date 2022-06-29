<template>
<div class="card">
  <div class="card__player">
    <div class="card__playerInner">
      <div  v-for=" (cards, index) in card"  :key="index" class="card__pItem" >
        <img class="card__zadn" src="../../assets/img/Zadn.png" alt="">
        <img class="card__jack" :src="require('../../assets/img/' + cards.Img)" alt="">
      </div>
    </div>
  </div>
  <div class="card__Opponent">
    <div class="card__OpponentInner">
      <div  v-for=" (opponents, index) in opponent"  :key="index" class="card__oItem" >
        <img class="card__zadn" src="../../assets/img/Zadn.png" alt="">
        <img class="card__jack" :src="require('../../assets/img/' + opponents.Img)" alt="">
      </div>
    </div>
  </div>
</div>
</template>

<script>
import './PlayerCard.css';
export default {
  name: "playerCard",
  props: ['card','opponent'],
}
</script>

<style scoped>
</style>

