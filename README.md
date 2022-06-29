# Карточная игра "21 очко" 
___
## 1. Страница карточной игры

![index](https://cdn.discordapp.com/attachments/356017746787565568/991735607644651560/unknown.png)
Все данные берутся из json файла rest-api.json где хранятся карточки выводимые в поле игроков.
В главном компоненте HomeView находится get запрос для api и главная функция которая выбирает рандомным образом карты, после чего вырезает из массива index той карты которую отрисовал, тем самым запрещая отрисовать ту же карту заного.Эта же функция используется для карт опонента из массива deckCard.

```javascript
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
}
```
Данная функция в методах отвечает за нажатие кнопки "Еще", сам оппонент берет карту из колоды когда у игрока больше очков чем у него.Так же тут отправляется колбек в функцию для подсчета очков каждого игрока.

```javascript
step(){
      this.random(this.playerCard);
      if(this.OpponentScore < this.PlayerScore){
        this.random(this.opponent);
        this.countScore('',this.opponent)
      }
      this.countScore(this.playerCard, '')
    }
```
Функция подсчета очков:

```javascript
countScore(playerScore,opponentScore){
      let score = 0;
      if(playerScore != ''){
        for ( let key in playerScore){
          score = playerScore[key].Score;
        }
        this.PlayerScore += score;
      }
      else if(opponentScore != ''){
        for ( let key in opponentScore){
          score = opponentScore[key].Score;
        }
        this.OpponentScore += score;
      }
    }
```

Нижняя кнопка "Хватит" подводит итог игры, если у игрока больше очков чем у оппонента, то он берет карты до того момента, пока его очки не станут превосходить игрока, после выходит окно с результатом игры.
```javascript
 Stop(){
      while (this.OpponentScore < this.PlayerScore) {
          this.random(this.opponent)
          this.countScore('', this.opponent)
      }
      if( this.PlayerScore < this.OpponentScore && this.OpponentScore > 21 ){
        this.message = 'Вы победили!';
        this.visibleMenu = true;

      }
      else{
        this.message = 'Вы проиграли!'
        this.visibleMenu = true;
      }
    }
```

Окно с результатом игры:

![Window](https://cdn.discordapp.com/attachments/356017746787565568/991741766048948285/unknown.png)

Так же стоят наблюдатели Watch, которые следят за очками игроков, после объявляют результат.
```javascript
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
```
