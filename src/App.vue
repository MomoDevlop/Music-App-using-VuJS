<template>
  <div id="app">
    <header>
      <h1>My Music </h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
           {{current.title}} -- <span>{{current.artist}}</span>
        </h2>
        <div class="control">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
          <h3>The playlist</h3>
          <button v-for="song in songs" :key="song.src"
          @click="play(song)"
          :value="song"
          :class="(song.src === current.src) ? 'song playing' : 'song'"
          >
           {{song.title }} --{{ song.artist}}

          </button>
      </section>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs:[

      {
        title: 'Boyfriend',
        artist: 'Justin Bieber',
        src: require("./assets/Justin_Bieber_Boyfriend.mp3")

      }
      ,{
        title: 'Love Me',
        artist: 'Justin Bieber',
        src: require("./assets/Justin_bieber_Love_me.mp3")

      }
      ,{
        title: 'One Time',
        artist: 'Justin Bieber',
        src: require("./assets/Justin_Bieber_One_Time.mp3")

      }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(son) {
        if (typeof son.src !== 'undefined') {
          this.current = son
          this.player.src = this.current.src

        }
        this.player.play()
        this.isPlaying = true
    },
    pause() {
      this.player.pause()
      this.isPlaying = false
    },
    next() {
      console.log('je suis la')
      this.index++;
      if (this.index > this.songs.length -1) {
        this.index = 0

      }
      this.current = this.songs[this.index]
      this.play(this.current)
    },
    async prev() {
      console.log('je suis la')
      await  this.isValid(this.index)
      this.index--;
      //await this.isValid(this.index)

      this.current = this.songs[this.index]
      this.play(this.current)
    },
    async isValid (index) {
      if (index <= 0) {
        this.index = this.songs.length
        return true

      }
      else{
        return false
      }


    }
  },
  // Initialized when the the Vue
  //
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
    //this.player.stop();
  },


}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: hsl(0, 27%, 94%);
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;

}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 25px;
  background-color: #212121;
  color: #fff;
}

.song-title {
  color: #212121;
  font-size: 32px;
  font-weight: 700px;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.control {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;

}
.play , .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 30px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #fff;
  background-color: #cc2e5d;
}
.next, .prev {
  font-size: 20px;
  font-weight: 200;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #fff;
  background-color: #ff5858;

}
.playlist {
  padding: 0px 30px;

}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 38px;
  text-align: center;

}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 300;
}

.playlist .song:hover{
  color: #ff5858;

}
.playlist .song.playing {
  color: #fff;
  background-image: linear-gradient(to right,#cc2e5d, #ff5858);
}
</style>
