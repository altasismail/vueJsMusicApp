<template>
  <div id="app">
    <header>
      <h1>
        Micael Jackson Musics
      </h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{current.title}}
          - <span>
            {{current.artist}}
          </span>
        </h2>
        <div class="controller">
          <button class="prev" @click="prev">
            Prev
          </button >
          <button class="play" v-if="!isPlaying" @click="play">
            Play
          </button>
          <button class="pause" v-else  @click="pause">
            Pouse
          </button>
          <button class="next" @click="next">
            Next
          </button>
        </div>
        
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
            {{song.title}} - {{song.artist}}
        </button>

      </section>
    </main>
    
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Black Or White',
          artist: 'Michael Jackson',
          src: require('./assets/Michael Jackson  Black Or White.mp3')
        },
        {
          title: 'Smooth Criminal',
          artist: 'Michael Jackson',
          src: require('./assets/Michael Jackson  Smooth Criminal.mp3')
        },
        {
          title: 'They Dont Care About Us',
          artist: 'Michael Jackson',
          src: require('./assets/Michael Jackson  They Dont Care About Us.mp3')
        },
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song){
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function(){
        this.index++;
        if (this.index > this.songs.length - 1){
        this.index = 0;
      }
        this.current = this.songs[this.index];
        this.play(this.current);

      }.bind(this));
      this.isPlaying = true;
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++;
      if (this.index > this.songs.length - 1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

    },
    prev(){
       this.index--;
      if (this.index < 0){
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);

    }
  },
  created (){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family:Arial, Helvetica, sans-serif

}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #fff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #212121;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-size: 24px;
  font-weight: 400;
  font-style: italic;
}

.controller {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}
button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

button:hover {
  background-color: #ad254e;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #cc2e5d;

}

.next, .prev {
  font-size: 14px;
  font-weight: 500;
  padding: 10px 20px;
  margin: 0 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #c54464
}
.playlist {
  padding: 0px 30px;

}
.playlist h3 {
  color: #212121;
  font-size: 20px;
  font-weight: 400px;
  margin-bottom: 28px;
  text-align: center;

}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 18px;
  font-weight: 500px;
  cursor: pointer;

}
.playlist .song.playing {
  color:#fff;
  background-image: linear-gradient(to right, #cc2e5d, #ff5858);

}
</style>
