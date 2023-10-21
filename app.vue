<template>
  <div class="mt-12">
    <div class="flex justify-center items-center">
      <div class="player max-w-min">
        <div class="header">
          <span> Music Player</span>
        </div>

        <div class="img">
          <img src="" id="thumb" />
        </div>
        <div class="details">
          <h3 id="title">{{ title }}</h3>
          <p id="musician"></p>
        </div>
        <!-- <audio src="assets/audio/audio1.mp3" controls autoplay loop></audio> -->

        <div class="time">
          <span id="start">{{ displayTotalTime }}</span>
          <span id="end">{{ remaining }}</span>
        </div>
        <input
          v-model="progress"
          @input="updateTime"
          type="range"
          id="progress"
        />
        <div class="action mt-4">
          <button @click.prevent="previousSound()">
            <i class="bx bx-skip-next">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="black"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M15.75 19.5L8.25 12l7.5-7.5"
                />
              </svg>
            </i>
          </button>

          <button @click.prevent="playSound()" class="play" id="play">
            <i v-if="!currentSong.paused" class="bx bx-play"
              ><svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M15.75 5.25v13.5m-7.5-13.5v13.5"
                />
              </svg>
            </i>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.348a1.125 1.125 0 010 1.971l-11.54 6.347a1.125 1.125 0 01-1.667-.985V5.653z"
              />
            </svg>
          </button>

          <button type="button" @click.prevent="nextSound()">
            <i class="bx bx-skip-next">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="black"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M8.25 4.5l7.5 7.5-7.5 7.5"
                />
              </svg>
            </i>
          </button>
        </div>
        <div class="flex justify-between items-center">
          <button>
            <svg class="h-6 w-6" width="800px" height="800px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M18 15L21 18M21 18L18 21M21 18H18.5689C17.6297 18 17.1601 18 16.7338 17.8705C16.3564 17.7559 16.0054 17.5681 15.7007 17.3176C15.3565 17.0348 15.096 16.644 14.575 15.8626L14.3333 15.5M18 3L21 6M21 6L18 9M21 6H18.5689C17.6297 6 17.1601 6 16.7338 6.12945C16.3564 6.24406 16.0054 6.43194 15.7007 6.68236C15.3565 6.96523 15.096 7.35597 14.575 8.13744L9.42496 15.8626C8.90398 16.644 8.64349 17.0348 8.29933 17.3176C7.99464 17.5681 7.64357 17.7559 7.2662 17.8705C6.83994 18 6.37033 18 5.43112 18H3M3 6H5.43112C6.37033 6 6.83994 6 7.2662 6.12945C7.64357 6.24406 7.99464 6.43194 8.29933 6.68236C8.64349 6.96523 8.90398 7.35597 9.42496 8.13744L9.66667 8.5" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </button>
          <button @click="volumeDown()">
            <i class="bx bx-skip-next">
              <svg :class="['h-6 w-6 rotate-180', volume === 0 ? 'opacity-40' : '']" width="800px" height="800px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M16 12.0001H22M4.6 9.00009H5.5012C6.05213 9.00009 6.32759 9.00009 6.58285 8.93141C6.80903 8.87056 7.02275 8.77046 7.21429 8.63566C7.43047 8.48353 7.60681 8.27191 7.95951 7.84868L10.5854 4.69758C11.0211 4.17476 11.2389 3.91335 11.4292 3.88614C11.594 3.86258 11.7597 3.92258 11.8712 4.04617C12 4.18889 12 4.52917 12 5.20973V18.7904C12 19.471 12 19.8113 11.8712 19.954C11.7597 20.0776 11.594 20.1376 11.4292 20.114C11.239 20.0868 11.0211 19.8254 10.5854 19.3026L7.95951 16.1515C7.60681 15.7283 7.43047 15.5166 7.21429 15.3645C7.02275 15.2297 6.80903 15.1296 6.58285 15.0688C6.32759 15.0001 6.05213 15.0001 5.5012 15.0001H4.6C4.03995 15.0001 3.75992 15.0001 3.54601 14.8911C3.35785 14.7952 3.20487 14.6422 3.10899 14.4541C3 14.2402 3 13.9601 3 13.4001V10.6001C3 10.04 3 9.76001 3.10899 9.54609C3.20487 9.35793 3.35785 9.20495 3.54601 9.10908C3.75992 9.00009 4.03995 9.00009 4.6 9.00009Z" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </i>
          </button>
          <button @click="volumeUp()">
            <i class="bx bx-skip-next">
              <svg :class="['h-6 w-6', volume === 1 ? 'opacity-40' : '']" width="800px" height="800px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M16 12.0001H22M19 9.00009V15.0001M4.6 9.00009H5.5012C6.05213 9.00009 6.32759 9.00009 6.58285 8.93141C6.80903 8.87056 7.02275 8.77046 7.21429 8.63566C7.43047 8.48353 7.60681 8.27191 7.95951 7.84868L10.5854 4.69758C11.0211 4.17476 11.2389 3.91335 11.4292 3.88614C11.594 3.86258 11.7597 3.92258 11.8712 4.04617C12 4.18889 12 4.52917 12 5.20973V18.7904C12 19.471 12 19.8113 11.8712 19.954C11.7597 20.0776 11.594 20.1376 11.4292 20.114C11.239 20.0868 11.0211 19.8254 10.5854 19.3026L7.95951 16.1515C7.60681 15.7283 7.43047 15.5166 7.21429 15.3645C7.02275 15.2297 6.80903 15.1296 6.58285 15.0688C6.32759 15.0001 6.05213 15.0001 5.5012 15.0001H4.6C4.03995 15.0001 3.75992 15.0001 3.54601 14.8911C3.35785 14.7952 3.20487 14.6422 3.10899 14.4541C3 14.2402 3 13.9601 3 13.4001V10.6001C3 10.04 3 9.76001 3.10899 9.54609C3.20487 9.35793 3.35785 9.20495 3.54601 9.10908C3.75992 9.00009 4.03995 9.00009 4.6 9.00009Z" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </i>
          </button>
          <button @click="repeatSong = !repeatSong">
            <svg :class="['h-6 w-6', repeatSong ? 'opacity-100' : 'opacity-60']" width="800px" height="800px" viewBox="0 0 24 24" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
              <title>Repeat-Play</title>
              <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="Repeat-Play">
                    <rect id="Rectangle" fill-rule="nonzero" x="0" y="0" width="24" height="24"> </rect>
                    <path d="M5,18.0002 L17,18.0002 C18.6569,18.0002 20,16.657 20,15.0002 L20,14" id="Path" stroke="#0C0310" stroke-width="2" stroke-linecap="round"></path>
                    <path d="M16,2 L19.2929,5.29289 C19.6834,5.68342 19.6834,6.31658 19.2929,6.70711 L16,10" id="Path" stroke="#0C0310" stroke-width="2" stroke-linecap="round"></path>
                    <path d="M8,14 L4.70711,17.2929 C4.31658,17.6834 4.31658,18.3166 4.70711,18.7071 L8,22" id="Path" stroke="#0C0310" stroke-width="2" stroke-linecap="round"></path>
                    <path d="M19,6 L7,6 C5.34315,6 4,7.34315 4,9 L4,10" id="Path" stroke="#0C0310" stroke-width="2" stroke-linecap="round"></path>
                  </g>
              </g>
            </svg>
          </button>
        </div>
      </div>
    </div>

    <div class="list">
      <div
        v-for="(song, index) in songsArray"
        @click="setCurrentSong(index)"
        class="border border-gray-300 player-list-item cursor-pointer"
        :class="currentIndex === index ? 'bg-red-400 text-white' : ''"
      >
        {{ song.title }}
      </div>
    </div>
      </div>
</template>
<script setup lang="ts">
import audio1 from "./assets/audio/Chaleya-Jawan-320-Kbps.mp3"
import audio2 from "./assets/audio/Hua-Main-Animal-320-Kbps.mp3"
import audio3 from "./assets/audio/Not-Ramaiya-Vastavaiya-Jawan-320-Kbps.mp3"
import audio4 from "./assets/audio/Pal Pal... Har Pal - Lage Raho Munna Bhai 128 Kbps.mp3"
import audio5 from "./assets/audio/_Ram Siya Ram_320(PagalWorld.com.pe).mp3"
import audio6 from "./assets/audio/_Meri Maa_320(PagalWorld.com.pe).mp3"
import audio7 from "./assets/audio/Manwa Laage 128 Kbps.mp3"
import audio8 from "./assets/audio/RaMChandra Kah Gaye - Gopi.mp3"


const title = ref("");
const currentIndex = ref(0);
const progress = ref(0);
const volume = ref(1);
const repeatSong = ref(false);

const songsArray = [
  { audio: audio1, title: "Chaleya Jawan" },
  { audio: audio2, title: "Hua Main Animal" },
  { audio: audio3, title: "Not Ramaiya Vastavaiya Jawan" },
  { audio: audio4, title: "pal pal pal har pal"},
  { audio: audio5, title: "Ram siya Ram"}, 
  { audio: audio6, title: "Meri maa"},
  { audio: audio7, title: "Manwa lage"},
  { audio: audio8, title: "Ram chandra kah gaye"}
];

const totalTime = ref(0);
const remaining = ref("0:00");

const currentSong = computed(() => {
  const song = new Audio(songsArray[currentIndex.value]!.audio);
  return song;
});

function formatTime(seconds: number) {
  const minutes = Math.floor(seconds / 60);
  const remainingSeconds = Math.floor(seconds % 60);
  return `${minutes}:${remainingSeconds < 10 ? "0" : ""}${remainingSeconds}`;
}

const displayTotalTime = computed(() => formatTime(totalTime.value));

function updateTime() {
  currentSong.value.currentTime = (totalTime.value * progress.value) / 100;
}

watch(
  currentSong,
  () => {
    if (currentSong) {
      currentSong.value.addEventListener("ended", () => {
        if(repeatSong.value) currentSong.value.play();
        else nextSound();
      });
      currentSong.value.addEventListener("loadeddata", () => {
        totalTime.value = currentSong.value.duration;
      });
      currentSong.value.addEventListener("timeupdate", () => {
        remaining.value = formatTime(currentSong.value.currentTime);
        progress.value =
          (currentSong.value.currentTime / currentSong.value.duration) * 100;
      });
      title.value = songsArray[currentIndex.value].title;
    }
  },
  { immediate: true }
);

function previousSound() {
  currentSong.value.pause();
  if (currentIndex.value === 0) currentIndex.value = 2;
  else currentIndex.value -= 1;
  playSound();
}

function playSound() {
  currentSong.value.paused
    ? currentSong.value.play()
    : currentSong.value.pause();
}

function nextSound() {
  currentSong.value.pause();
  if (currentIndex.value === songsArray.length - 1) currentIndex.value = 0;
  else currentIndex.value += 1;
  playSound();
}

const volumeUp = () => {
  if (volume.value < 1) {
    volume.value = Number((volume.value + 0.1).toFixed(1));  
  }
};

const volumeDown = () => {
  if (volume.value > 0) {
    volume.value = Number((volume.value - 0.1).toFixed(1));
  }
};

function setCurrentSong(index: number) {
  if(!currentSong.value.paused) currentSong.value.pause();
  currentIndex.value = index;
  currentSong.value.play()
}

// update current song's volume on volume change.
watch(volume, () => {
  currentSong.value.volume = volume.value;
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

.bottom  {
  filter: invert(0.23);
}

.player {
  width: 90%;
  max-width: 300px;
  height: 480px;
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
