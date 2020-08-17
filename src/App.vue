<template>
  <div id="app">
    <header>
      <h2>My Music</h2>
    </header>
    <main>
      <section v-if="current" class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button @click="prev" class="prev">Prev</button>
          <button @click="play" v-if="!isPlaying" class="play">Play</button>
          <button @click="pause" v-else class="pause">Pause</button>
          <button @click="next" class="next">Next</button>
        </div>
      </section>

      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="song.src == current.src ? 'song playing' : 'song'">{{ song.title }} - {{ song.artist }}</button>
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
        songs: [
          {
            title: 'coulda woulda',
            artist: 'celine dion',
            src: require('./assets/celine dion-coulda w.mp3')
          },
          {
            title: 'if i let you go',
            artist: 'WestLife',
            src: require('./assets/westlife- if i let you go.mp3')
          }
          // {
          //   title: 'white Flag',
          //   artist: 'Dido',
          //   src: require('./assets/Dido - White Flag 1.mp3')
          // }
        ],
        player: new Audio()
      };
    },
    methods: {
      play(song) {
        if (typeof song.src != 'undefined') {
          this.current = song;
          this.player.src = this.current.src;
        }
        this.player.play();
        this.isPlaying = true;
        this.player.addEventListener(
          'ended',
          function() {
            this.index++;

            if (this.index > this.songs.length - 1) {
              this.index = 0;
            }
          }.bind(this)
        );
      },
      pause() {
        this.player.pause();
        this.isPlaying = false;
      },
      next() {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      },
      prev() {
        this.index--;
        if (this.index < 0) {
          this.index = this.songs.length - 1;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }
    },
    created() {
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
    }
  };
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    font-family: sans-serif;
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
    width: 100;
    max-width: 769px;
    margin: 0 auto;
    padding: 25px;
  }

  .song-title {
    color: #54565a;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
  }
  .song-title span {
    color: #54565a;
    font-size: 25px;
    font-weight: 400;
    font-style: italic;
  }

  .controls {
    display: flex;
    justify-content: center;
    padding: 30px 15px;
    align-items: center;
  }
  button {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
  }
  .play,
  .pause {
    font-size: 20px;
    font-weight: 700;
    padding: 15px 25px;
    margin: 0px 15px;
    border-radius: 8px;
    color: #fff;
    background-color: rgb(68, 7, 124);
  }
  .play:hover,
  .pause:hover {
    background-color: rgb(113, 23, 197);
  }

  .next,
  .prev {
    font-size: 16px;
    font-weight: 700;
    padding: 10px 20px;
    margin: 0px 15px;
    border-radius: 6px;
    color: #fff;
    background-color: rgb(41, 23, 58);
  }

  .playlist {
    padding: 0px 30px;
  }
  .playlist h3 {
    color: #212121;
    font-size: 1.8rem;
    font-weight: 400;
    margin-bottom: 30px;
    text-align: center;
  }

  .playlist .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    color: rgb(41, 23, 58);
  }

  .playlist .song.playing{
    color: white;
    background-image:linear-gradient(to right, rgb(41, 23, 58), rgb(113, 23, 197) ) ;
  }
    .playlist .song:hover{
  background-color: rgb(68, 7, 124);
  color: white;
  }
</style>
