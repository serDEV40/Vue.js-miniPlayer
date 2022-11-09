<style scoped>
  *{
    padding: 0px;
    margin: 0px;
    box-sizing: border-box;
    font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-weight:500;
    letter-spacing: -1px;
  }
  .card{
    position: absolute;
    width: 350px;
    background-color: rgb(0,0,0,0.75);
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }

  .myButton{
    font-size: 1rem;
    font-weight: 500;
    padding: 0.5rem 0.75rem;
    border-radius: 0.25rem;
    border:none;
    background-color: #ffc107;
  }

  .myButton:hover{
    cursor: pointer;
  }

  .mt{
    margin-top: 0.25rem;
  }

  .d-flex{
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
</style>


<template>
  <div class="card">
    <div class="card-img" style="padding:0px;margin:0px;overflow:hidden">
      <img src="https://preview.redd.it/m5fcobbr4gj71.png?width=640&crop=smart&auto=webp&s=17726aae4764d7f21a845c96532a833912ad064e" style="width:-webkit-fill-available" alt="">
    </div>
    <div class="card-body" style="padding: 0.5rem 1rem;">
      <div class="album d-flex mt" style="color:white">
        <span>
          {{songName}}
        </span>
        <span>
          {{songWter}}
        </span>
      </div>
      <div class="mt">
        <div>
          <input type="range" style="width:100%;" name="" id="">
        </div>
        <div class="d-flex" style="color:white;margin-bottom:0.5rem;">
          <span>{{cTime}}</span>
          <span>{{dTime}}</span>
        </div>
      </div>
      <div class="btn-group d-flex mt">
        <button @click="Before" class="myButton">Geri</button>
        <button @click="Play" class="myButton">Durdur / Oynat</button>
        <button @click="Next" class="myButton">İleri</button>
      </div>
    </div>
  </div>
</template>


<script>
  export default {
    data(){
      return{
        songName : null,
        songWter : null,
        dTime : null,
        cTime : null,
        audio : null,
        index : 0,
        items : [
          {
            artist : "Jeremy Soule",
            singName : "Kyne's Peace",
            location : "../musics/Kynes Peace.mp3",
          },{
            artist : "Alt-J",
            singName : "Taro",
            location : "../musics/Taro.mp3",
          },{
            artist : "Emis-Killa",
            singName : "CULT",
            location : "../musics/Emis Killa - CULT.mp3",
          }
        ]
      }
    },
    methods: {
      Before(){
        var accord = this.index--;
        if(this.index < 0){
          accord = this.items.length - 1;
          this.displayMusic(accord);
        }else if(this.index > 0){
          accord--;
          this.displayMusic(accord);
        }
      },
      Play(){
        if( this.audio.paused ){
          this.audio.play();
        } else if( this.audio.played ){
          this.audio.pause();
        }
      },
      Next(){
        var elixir = this.index++;
        
        if(elixir >= this.items.length){
          this.index = 0;
          this.displayMusic(this.index);
          console.log(this.items);
        } else{
          this.displayMusic(elixir);
        }
      },
      displayMusic(object){
        this.audio.src = this.items[object].location;
        this.songName = this.items[object].singName;
        this.songWter = this.items[object].artist;
      },
      calculateTime(event){
        var minute = Math.floor(event / 60);
        var second = Math.floor(event % 60);

        if(minute < 10){
          minute = "0" + minute;
        }
        if(second < 10){
          second = "0" + second;
        }

        return(minute + ":" + second);

      }
    },
    created(){
      var vm = this;
      this.audio = new Audio();
      this.audio.preload = "none";
      this.audio.src= this.items[this.index].location;
      this.songName = this.items[this.index].singName;
      this.songWter = this.items[this.index].artist;
      this.audio.onloadedmetadata = function(){
        vm.dTime = vm.calculateTime(vm.audio.duration);
      };
      this.audio.ontimeupdate = function(){
        vm.cTime = vm.calculateTime(vm.audio.currentTime);
      }
    }
  }
</script>