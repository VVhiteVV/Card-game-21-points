<template>
  <div class="home">
    <player-card class="home__playerCard" :deck-cards="deckCard" :opponent="opponent" :card="playerCard" />
    <div class="home__deckCard">
      <div class="home__score">
        <div class="home__pScore"><p>{{PlayerScore}}</p></div>
        <div class="home__oScore"><p>{{OpponentScore}}</p></div>
      </div>
      <div class="home__deck">
        <img :src="require('../../assets/img/Zadn.png')" class="home__deckItem" width="195" alt="">
        <button @click="step" class="home__btnStill" >Еще</button>
        <button @click="Stop" class="home__btnStop" >Хватит</button>
      </div>
    </div>
    <menu-game :visible-menu="visibleMenu" :message="message" :o-scrore="OpponentScore" :p-score="PlayerScore" />
  </div>
</template>
<script>
import axios from 'axios';
import './HomeView.css';
import menuGame from "@/components/menu/menu";
import playerCard from "@/components/PlayerCard/PlayerCard";

export default {
  name: 'HomeView',
  components: {
    playerCard,
    menuGame
  },
  data(){
    return {
      deckCard: [],
      opponent: [],
      playerCard: [],
      IndexCard: ' ',
      visibleMenu: false,
      message: '',
      Validate: 50,
      PlayerScore: 0,
      OpponentScore: 0
    }
  },
  created() {
    axios.get('http://localhost:3000/users').then((response) => {
        this.deckCard = response.data;
        });
    console.log(this.deckCard)
  },
  methods: {
    step(){
      this.random(this.playerCard);
      if(this.OpponentScore < this.PlayerScore){
        this.random(this.opponent);
        this.countScore(this.opponent)
      }
      this.countScore(this.playerCard)
    },
    Stop(){
      while (this.OpponentScore < this.PlayerScore) {
          this.random(this.opponent)
          this.countScore(this.opponent)
      }
      if( this.PlayerScore < this.OpponentScore && this.OpponentScore > 21 ){
        this.message = 'Вы победили!';
        this.visibleMenu = true;

      }
      else{
        this.message = 'Вы проиграли!'
        this.visibleMenu = true;
      }
    },
    countScore(ScorePlayers){
      let score = 0;
      if(ScorePlayers === this.playerCard){
        for ( let key in ScorePlayers){
          score = ScorePlayers[key].Score;
        }
        this.PlayerScore += score;
      }
      else if(ScorePlayers === this.opponent){
        for ( let key in ScorePlayers){
          score = ScorePlayers[key].Score;
        }
        this.OpponentScore += score;
      }
    },
    stopTimer(){
      location.reload();
    },
    random(deck){
      this.IndexCard = Math.ceil(Math.random()*this.Validate);
      for( let key in this.deckCard){
        if(key == this.IndexCard){
          deck.push(this.deckCard[key]);
          this.deckCard.splice(this.IndexCard,1);
        }
      }
      this.Validate--;
      }
    },
  watch:{
    PlayerScore: function (){
        if(this.PlayerScore > 21){
          this.message = 'Вы проиграли!'
          this.visibleMenu = true;

        }
        else if(this.PlayerScore === 21){
          this.message = 'Вы победили!'
          this.visibleMenu = true;
        }
      },
    OpponentScore: function (){
      if(this.OpponentScore > 21){
        this.message = 'Вы победили!';
        this.visibleMenu = true;
      }
    }
    }
}
</script>
