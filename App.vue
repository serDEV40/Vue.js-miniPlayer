<template>
  <div class="card bg-dark text-white" style="width:18rem">
    <div class="card-body">
      <div class="text d-flex align-items-center justify-content-between">
        <span>{{songName}}</span>
        <span>{{songWter}}</span>
      </div>
      <div class="mt-2 d-flex align-items-center justify-content-between">
        <button class="btn btn-warning" @click="Back"> Geri </button>
        <button class="btn btn-warning" @click="Play"> Oynat || Durdur </button>
        <button class="btn btn-warning" @click="Next"> => </button>
      </div>
      <div class="mt-1 pt-1 d-flex align-items-center">
        <div style="font-size:1.5rem;height:36px;width:33px">{{speaker}}</div>
        <input class="mx-2" type="range" min="0" @input="Volume" max="100" style="width:calc(100% - 73px)">
        <div class="card-text" style="height:24px;width:40px">{{volume}}</div>
      </div>
    </div>
    <span>{{index}}</span>
  </div>
</template>


<script>
  export default {
    data(){
      return{
        songName : null,
        songWter : null,
        source : null,
        dTime : null,
        cTime : null,
        audio : null,
        volume : null,
        speaker : null,
        index : 0,
        items : [
          {
            artist : "Emis-Killa",
            singName : "CULT",
            location : "../musics/Emis Killa - CULT.mp3",
          },{
            artist : "Jeremy Soule",
            singName : "Kyne's Peace",
            location : "../musics/Kynes Peace.mp3",
          },{
            artist : "Alt-J",
            singName : "Taro",
            location : "../musics/Taro.mp3",
          },
        ]
      }
    },
    methods: {
      Playing(){
        this.audio.classList.remove("musicStopped");
        this.audio.classList.add("musicPlaying");
        this.audio.play();
      },

      Paused(){
        this.audio.classList.add("musicStopped");
        this.audio.classList.remove("musicPlaying");
        this.audio.pause();
      },

      Volume(event){
        var Volume = event.target.value;
        if(Volume == 0){
          this.audio.muted = true;
          this.volume = "%"+Volume;
          this.speaker = "🔇";
        }else if( Volume > 0 ){
          this.audio.muted = false;
          this.audio.volume = Volume / 100;
          if(this.audio.volume == 1){
            this.volume = 1;  
          }else if(this.audio.volume < 1){
            this.volume = this.audio.volume;
            // this.audio.volume = "0." + Volume;
          }
          this.speaker = "🔉"
        }
      },

      Information(){
        this.audio.src = this.items[this.index].location;
        this.volume = (this.audio.volume);
        this.songName = this.items[this.index].singName;
        this.songWter = this.items[this.index].artist;
      },
      
      Play(){
        if(this.audio.classList.contains("musicStopped")){
          this.Playing();
        }else if(this.audio.classList.contains("musicPlaying")){
          this.Paused();
        }
      },

      Next(){
        this.index = this.index + 1;
        if(this.index > this.items.length - 1){
          this.index = 0;
          this.Information(); 
        }else {
          this.Information();
        }
      },

      Back(){
        this.index = this.index - 1;
        if(this.index < 0){
          this.index = this.items.length - 1;
          this.Information(); 
          this.audio.play()
        }else {
          this.Information();
          this.audio.play();
        }
        console.log(this.index);
      }
    },
    created(){
        this.index = 0;
        this.audio = new Audio();
        this.audio.preload = "none";
        this.audio.classList.add("musicStopped");
        this.Information();
        this.speaker = "🔉";
      }
  }
</script>
