<template>
  <div id="app" class="music-app">
    <Header />
    <main class="music-container">
      <div class="music__title">
        {{ current.title }} - <span>{{ current.artist }}</span>
      </div>

      <div class="music__control">
        <button class="music-prev">
          <img :src="icons[0].prev" @click="prev">
        </button>
        <button class="music-play" v-if="!isPlaying" @click="play">
          <img :src="icons[1].play">
        </button>
        <button class="music-pause" v-else @click="pause">
          <img :src="icons[2].pause">
        </button>
        <button class="music-next">
          <img :src="icons[3].next" @click="next">
        </button>
      </div>

      <div class="music__list">
        <h3>The playlist</h3>
        <music-list
          v-for="(song, index) in songs"
          :key="index"
          :song=song
          @play="play"
          :class="(song.src == current.src) ? 'music__item music__item-playing' : 'music__item'"
        ></music-list>
      </div>
    </main>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import MusicList from './components/MusicList.vue';

export default {
  name: 'App',
  data() {
    return {
      icons: [
        { prev: require('./assets/icons/previous.svg') },
        { play: require('./assets/icons/play.svg') },
        { pause: require('./assets/icons/pause.svg') },
        { next: require('./assets/icons/next.svg') }
      ],
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'BadLiar',
          artist: 'ImagineDragons',
          src: require('./assets/BadLiar-ImagineDragons.mp3')
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require('./assets/deaf-kev-invincible.mp3')
        },
        {
          title: 'GirlsLikeYou',
          artist: 'Maroon5CardiB',
          src: require('./assets/GirlsLikeYou-Maroon5CardiB.mp3')
        },
        {
          title: 'IMNotHer',
          artist: 'ClaraMae',
          src: require('./assets/IMNotHer-ClaraMae.mp3')
        },
        {
          title: 'LetHerGo',
          artist: 'Passenger',
          src: require('./assets/LetHerGo-Passenger.mp3')
        },
        {
          title: 'LoveYourself',
          artist: 'JustinBieber',
          src: require('./assets/LoveYourself-JustinBieber.mp3')
        },
        {
          title: 'Maps',
          artist: 'Maroon5',
          src: require('./assets/Maps-Maroon5.mp3')
        },
        {
          title: 'neffex',
          artist: 'grateful',
          src: require('./assets/neffex-grateful.mp3')
        },
        {
          title: 'NoGuidance',
          artist: 'ChrisBrownDrake',
          src: require('./assets/NoGuidance-ChrisBrownDrake.mp3')
        },
        {
          title: 'SoFarAwayAcoustic',
          artist: 'AdamChristopher',
          src: require('./assets/SoFarAwayAcoustic-AdamChristopher.mp3')
        },
        {
          title: 'ThucGiac',
          artist: 'DaLAB',
          src: require('./assets/ThucGiac-DaLAB.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        console.log(this.current);

        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener('ended', function() {
        this.next();
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
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
  },
  components: {
    Header,
    MusicList
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
	font-family: sans-serif;
}

.music-app {
  background-image: url('./assets/icons/bg.jpeg');
  background-size: cover;
  background-position: bottom;
}

.music-container {
  display: flex;
  flex-direction: column;
  align-items: center;

  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

/* Title  */
.music__title {
  color: #ff0000;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
  padding: 20px;
}

.music__title span {
  font-weight: 400;
  font-style: italic;
}

/* button  */
.music__control {
  display: flex;
  justify-content: center;
}

img {
  width: 20px;
  height: 20px;
}

.music-play img,
.music-pause img {
  width: 50px;
  height: 50px;
}

.music__control button {
  border: none;
  background: none;
  margin: 0 20px;
  padding: 3pxs;
}

.music__control button:hover {
  cursor: pointer;
}

/* list  */
.music__list {
  display: flex;
  flex-direction: column;
  align-items: center;

  padding-bottom: 30px;
}

h3 {
  color: #fff;
  font-size: 28px;
  font-weight: 700;
  padding: 30px 0 20px;
  text-align: center;
}

.music__item {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 400;
  cursor: pointer;

  background: none;
  color: rgb(197, 197, 197);
  border: none;
  border-bottom: 1px solid rgb(94, 94, 94);
}

.music__item:hover {
  color: #fff;
}

.music__item-playing {
  color: #FFF;
  font-weight: 700;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>
