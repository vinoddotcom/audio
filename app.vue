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
          <div>
            <img class="hover:invert[.25]" width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAACPUlEQVR4nO2ZPWgUQRTHfyKaXDgxlYXkrsnFTtFoIyadxUm0Fyy0ESsljaYTY7qUokjAj1pQtLNRU2jtRydeQJJGtDgR9OK3MrALf4bMZJfZOzPn/mCb99699/7L3uybWSgpKSn5H2gAj4FPwBOgTkTsAd4Bf+QyYqLgINC2mjfXdyLgMPBZmv5oidjQHANWpdn3wN5YBJwEfkijy8CuxJdHwG7gOLCVHnIO+C1NvgZq4s8qYCfQSeJu0iMuWg0+B3ZYMVkFjErcL2C8i32zDbhiNfcUGF4jNs8jdFdiF/M2VQFmgSXg5xrLoO96CAw58uYR0AC+SfyBPM0/y9l0et1Z50/nElAFrgNXk/optyT+WlYBszmbNnfpJXAa2LRObpeAs2KfE/sh6z0ymEXAkvzoPLCF4nAJmBL7B2AgsZsb8kZ8k1mK6DNf9BrsErAZeCu+I+JbEPtMSJEi8OWeF59Z1VJOif1BaJFQfLmb1jKcsk/sL0KLhOLLPSI+M36n1MS+ElokFF/uAfF9FXtF7J3QIqH4clccjQ6J/UtokVB8uWuxP0LN2P/E8xmW0fuxv8gu5B0lZgoW4RJw1DNKtMQ3kaXIJcfQ5hvmXgFnAoa5abFfFvuE2NsibMON09uTreOCNU7flngzamdmMNkitv7hhmbM2tDsp0tUu7SlvNfrk7wiN/Vj1qbevAuiOlapJ/OQibtBpAdb48CJXh9s9cXRYl8c7vbF8brvA8cjImM0abod4yemkpKSEpz8BYvNmvdM2SAEAAAAAElFTkSuQmCC">
          </div>
          <div>
            <img width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAACHUlEQVR4nO2YO0hcQRSGPxMfBA1oAgqmMGgIIloIBsEiRNRgoTZpxAhi46MxlY9GDKQJgVSplIRlIaQMxEoIQUERrGIhpLJQEQmKoBBfeRgGZuEwXPcu7CzOwHxw2GXP4ez/3509c+dCIBAIBDygCegEyvCMu8ACcKnjCOjCE+qBTSE+FYdAMY7TDRxHiE9FM46SB0wBf4VYZaQXWBefPcFBioCkcaU39VJSLLpsoBJYM8QvA+WixlkDjcCWIX4WKDDqnDTwHDgVwi6A0Stq4wzcAGqAm7kQqpbCCDApYs646gdAa5oecQaSOvdNDwNrdAD7aUaiig2gOqZPnIHtXCyx8gzEzwO3M+gVZ+CdyCdsGRgWTXeAN8BrEf167WLBwCOR37O1jGZE01dZ9srkT3wgau5jgZeioXqfDZmM0UVRo+5evTPwSdT04aGBhKgZxEMDn0XNMzw0sCFq1FTyysAd4I/Oq9cSPDMwIPKrWGJaNH2bYwNLIj+OJXpF03Mt4quID0CFJQM/dO7UOEdkRaFxFIyKXeCxBQPN+oI8xTJVxpdHxW9gIub+5doPNA+BNqBdhzrM/DSMfAFKXTUQxT1gxTCxdcVjEycNKPL1iP0nBJ4BL/DEQIoe/dRN/hofxVM45w0oHgDfI46btXpzct6A4hbwPmZqteABA8CvCPEnaSaVczSIHTa1XwzhGYX63n4MqLtuMYFAIBBA8h8+ZyJf0yyVdAAAAABJRU5ErkJggg==">
          </div>
          <div>
            <img width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAACOElEQVR4nO2YTWsTQRiAn40lotJDQPAaqOhZDT3Y5Bd46a3/QGjBcz2rKV6sqYfa4EeQpvbiyYPX5lJDfkBzUONV0XqQSGyRprLwLgxDNrub7NfQfWAuO/O+8w47835BRkZGRgRUgT7wDMhhIL+BUxmvTDzEunIAIw9hAc+1Q7w28RCbZ+oQa8CxttgZR0BPhMshGlgRnT3Z49Tn2BilrB9AwVvg0hSG27K7AfbTh+2lRvpetz8wanwEZicwflZkJzXe9Q+M4zxwE2gAQ0XRB2AmgJ4ZkXHkh3KFbsgeOjlxoaG61BVNYT2A7JYmuzxmbSTGOzzSFK/6kLmvyTxIMg7YGzS1q7A0Zv2SdvW2RYcbtTgisX1nW8omf11cbFnmnHV7QN5D9yCuNKIAdJXNOtr8NeBQmT8QGS8eS1x4GkfkLQLfgV/AdeX7ZeCTYvwP4CopZV6iqsMFoK0Y/wcoYQg54J1i/AmwiMG5/D0M4q5m/BMM4g7wTzH+PXAOQyjJQ3WMb8tDdqjIQ08lReCbYvxX4IoyPycu9Gca3WhBgpNj/KEEL5WOMt/1GciqcbRV8pIWBE0lWi6pc6xtFUsSskmTuaZHMrcedTL3MIR02taRSDqtFzR2keKXuia7EldBY9/ZW8AbTWEYJWVDytWpS8o17bF5jf00FfVWgLbKUB7wRaZrq+xE0VZx+wN2cfEZeAHcJjwWgJfAlzAaW2nEMrnJa2XGJ0gt6kgcNQOTjY+9rZKRkUFy/AdXR3v4IGsOegAAAABJRU5ErkJggg==">
          </div>
          <div>
            <img width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAABRklEQVR4nO2YQU7DMBBFH1mUW1Rwh94AlZMVwYYCRWrVBZwN1AMEqIpYVBpk5EqW1So1TWMPzJO8TPKfJ3HsAcMwSuEKWAIToEIh74D48aRRYhwIqJQ4AWaRxLNGialJBJwBc2ABrKOZ6Xo8kMgl8JE5tATDrVJ7c15YeEmtwDy48AW4AHp0Q+WX0IOW1EVwsQuPpvBEH6yqmd8Q3qQr7hvCD0oX+GwIX5cucAN8AXc7wkvpArvYhFcrICaQGbEKZEasApmRf12B+ni5ji9QRxso93t/9M2nUfs52xXYFj7c6r5RuMCgYZ9+S3fIId9jq4eMX3AaPNcdslSFdwyjs7ma8D3f1nkNnu86dnszKaCNIlFPqJ8isCws/DC1hLkrsPavzzR15v9cq3tmEgVQbVlSXesDzRIrFFL5mXfNp+vcYQzD4IdvfiKXOauBYKcAAAAASUVORK5CYII=">
          </div>
        </div>
      </div>
    </div>

    <div class="list">
      <div
        v-for="(song, index) in songsList"
        @click="setCurrentSong(index)"
        class="border border-gray-300 player-list-item"
        :class="currentIndex === index ? 'bg-red-400 text-white' : ''"
      >
        {{ getName(song) }}
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import audio1 from "./assets/audio/Chaleya-Jawan-320-Kbps.mp3";
import audio2 from "./assets/audio/Hua-Main-Animal-320-Kbps.mp3";
import audio3 from "./assets/audio/Not-Ramaiya-Vastavaiya-Jawan-320-Kbps.mp3";

const songsList = ref([audio1, audio2, audio3]);

function getName(url: string) {
  return url.split("/").pop();
}

const title = ref("");
const currentIndex = ref(0);
const progress = ref(0);

const songsArray = [
  { audio: audio1, title: "Chaleya Jawan" },
  { audio: audio2, title: "Hua Main Animal" },
  { audio: audio3, title: "Not Ramaiya Vastavaiya Jawan" },
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
        nextSound();
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

function setCurrentSong(index: number) {
  if(!currentSong.value.paused) currentSong.value.pause();
  currentIndex.value = index;
  currentSong.value.play()
}

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
