<template>
  <div class="mt-12">
    <div class="flex  justify-center items-center">
      <div class="player max-w-min">
        <div class="header ">
          <span> Music Player</span>
        </div>

        <div class="img">
          <img src="" id="thumb">
        </div>
        <div class="details">
          <h3 id="title"></h3>
          <p id="musician"></p>
        </div>
        <!-- <audio src="assets/audio/audio1.mp3" controls autoplay loop></audio> -->

        <div class="time">
          <span id="start">2:28</span>
          <span id="end">4:33</span>
        </div>
        <input type="range" id="progress" value="0">

        <div class="action bg-gray-100">

          <button @click.prevent="previousSound()">
            <i class="bx bx-skip-next">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="black"
                class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
              </svg>
            </i>
          </button>

          <button @click.prevent="playSound()" class="play" id="play">
            <i class="bx bx-play"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
              stroke="currentColor" class="w-6 h-6">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
            </svg>
          </i>
          </button>

          <button type="button" @click.prevent="nextSound()">
            <i class="bx bx-skip-next">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="black"
                class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
              </svg>
            </i>
          </button>


        </div>
        <div class="action pb-12">
          <button onclick="volumeUp()">
            <i class="bx bx-skip-next">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="black"
                class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5L8.25 12l7.5-7.5" />
              </svg>
            </i>
          </button>
          <button onclick="volumeUp()">
            <i class="bx bx-skip-next">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="black"
                class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 4.5l7.5 7.5-7.5 7.5" />
              </svg>
            </i>
          </button>
        </div>

      </div>

    </div>

    <div class="list">
      <div 
        v-for="song in songsList"
       class="border border-gray-300 player-list-item"
       :class="currentSong?.attributes.src.nodeValue === song ? 'bg-red-400 text-white' : ''">{{getName(song)}}</div>
    </div>
  </div>
</template>
<script setup lang="ts">
import audio1 from "./assets/audio/Chaleya-Jawan-320-Kbps.mp3"
import audio2 from "./assets/audio/Hua-Main-Animal-320-Kbps.mp3"
import audio3 from "./assets/audio/Not-Ramaiya-Vastavaiya-Jawan-320-Kbps.mp3"

const songsList = ref([audio1, audio2, audio3]);

function getName(url: string) {
  return url.split("/").pop()
}

const volume = ref(1);
const myVideo = ref(null);

const volumeUP = () => {
  if (volume.value < 1) {
    volume.value += 0.1;
  }
};

const volumeDown = () => {
  if (volume.value > 0) {
    volume.value -= 0.1;
  }
};

const currentIndex = ref(1)
const songsArray = [null, audio1, audio2, audio3]
const currentSong = ref<HTMLAudioElement | null>(null)

const count = ref(0)

function previousSound() {
  if ((currentIndex.value - 1) === 0) currentIndex.value = 3;
  else currentIndex.value -= 1;
  count.value = 0
  playSound()
}

function playSound() {
  if (currentSong.value) currentSong.value.pause()
  const song = new Audio(songsArray[currentIndex.value]!)
  currentSong.value = song
  if (count.value % 2 === 0) {
    song.play();
    count.value += 1;
  }
  else {
    song.pause();
    count.value += 1;
  }
}

function nextSound() {
  if ((currentIndex.value + 1) === 4) currentIndex.value = 1;
  else currentIndex.value += 1;
  count.value = 0
  playSound()
}


watch(currentSong, () => {
  console.dir(currentSong.value?.attributes.src.nodeValue, "==")
})
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,500;0,600;1,400;1,600&display=swap");

* {

  padding: 0;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  font-family: "Josefin Sans", sans-serif;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: url("https://img.freepik.com/free-vector/3d-shapes-background-purple-abstract-gradient-liquid-shapes-vector_53876-157845.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
}

.player {
  width: 90%;
  max-width: 300px;
  height: 430px;
  overflow: hidden;
  border-radius: 30px;
  padding: 20px;
  box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.05);
  position: relative;
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(8.5px);
  -webkit-backdrop-filter: blur(8.5px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.header {
  text-align: center;
  margin: 0px auto;
  padding: 5px;
  border-bottom: 1px solid #f5f5f5;
  font-size: 20px;
  font-weight: bold;
  color: #fff;
}

.header span {
  color: #ff688f;
}

.img {
  width: 160px;
  height: 160px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px auto;
  border-radius: 50%;
  background: linear-gradient(120deg, #ffa17f, #ff688f);
  overflow: hidden;
  margin-top: 15px;
}

.img img {
  width: 150px;
  height: 150px;
  display: block;
  margin: 20px auto;
  border-radius: 50%;
  object-fit: cover;
  z-index: 9999;
}

.img img.play {
  animation: 15s rotate linear infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

.details {
  text-align: center;
}

.details p {
  color: #777;
  font-size: 15px;
  margin-top: 5px;
}

.action {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  justify-content: center;
  align-items: center;
}

.action button {
  background: #fff;
  box-shadow: 5px 5px 10px 5px rgba(0, 0, 0, 0.028);
  border-radius: 50%;
  padding: 10px;
  font-size: 28px;
  width: 60px;
  height: 60px;
  border: none;
  outline: none;
  margin: 10px;
  border: 2px solid #f8f8f8;
  cursor: pointer;
  color: #ddd;
  display: flex;
  justify-content: center;
  align-items: center;
}

.action button.play {
  background: linear-gradient(135deg, #ffa17f, #ff688f);
  color: #fff;
}

.time {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 20px auto 10px auto;
  width: 90%;
  color: #777;
}

#progress {
  margin: 0px auto;
  display: block;
  width: 90%;
  -webkit-appearance: none;
  height: 6px;
  background: #f7f7f7;
}

#progress::-webkit-slider-thumb {
  background: red;
  -webkit-appearance: none;
  width: 12px;
  height: 12px;
  border-radius: 3px;
  background: linear-gradient(135deg, #ffa17f, #ff688f);
  transition: 0.2s ease-in-out;
}

.list {
  margin-top: 20px;
  padding: 10px;
  width: 100%;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}
.player-list-item {
  padding: 10px;
  border-radius: 5px;
  margin-top: 5px;
}

</style>