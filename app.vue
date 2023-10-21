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
            <img width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAACOElEQVR4nO2YTWsTQRiAn40lotJDQPAaqOhZDT3Y5Bd46a3/QGjBcz2rKV6sqYfa4EeQpvbiyYPX5lJDfkBzUONV0XqQSGyRprLwLgxDNrub7NfQfWAuO/O+8w47835BRkZGRgRUgT7wDMhhIL+BUxmvTDzEunIAIw9hAc+1Q7w28RCbZ+oQa8CxttgZR0BPhMshGlgRnT3Z49Tn2BilrB9AwVvg0hSG27K7AfbTh+2lRvpetz8wanwEZicwflZkJzXe9Q+M4zxwE2gAQ0XRB2AmgJ4ZkXHkh3KFbsgeOjlxoaG61BVNYT2A7JYmuzxmbSTGOzzSFK/6kLmvyTxIMg7YGzS1q7A0Zv2SdvW2RYcbtTgisX1nW8omf11cbFnmnHV7QN5D9yCuNKIAdJXNOtr8NeBQmT8QGS8eS1x4GkfkLQLfgV/AdeX7ZeCTYvwP4CopZV6iqsMFoK0Y/wcoYQg54J1i/AmwiMG5/D0M4q5m/BMM4g7wTzH+PXAOQyjJQ3WMb8tDdqjIQ08lReCbYvxX4IoyPycu9Gca3WhBgpNj/KEEL5WOMt/1GciqcbRV8pIWBE0lWi6pc6xtFUsSskmTuaZHMrcedTL3MIR02taRSDqtFzR2keKXuia7EldBY9/ZW8AbTWEYJWVDytWpS8o17bF5jf00FfVWgLbKUB7wRaZrq+xE0VZx+wN2cfEZeAHcJjwWgJfAlzAaW2nEMrnJa2XGJ0gt6kgcNQOTjY+9rZKRkUFy/AdXR3v4IGsOegAAAABJRU5ErkJggg==">
          </div>
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
          <div>
            <img width="30" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAABRklEQVR4nO2YQU7DMBBFH1mUW1Rwh94AlZMVwYYCRWrVBZwN1AMEqIpYVBpk5EqW1So1TWMPzJO8TPKfJ3HsAcMwSuEKWAIToEIh74D48aRRYhwIqJQ4AWaRxLNGialJBJwBc2ABrKOZ6Xo8kMgl8JE5tATDrVJ7c15YeEmtwDy48AW4AHp0Q+WX0IOW1EVwsQuPpvBEH6yqmd8Q3qQr7hvCD0oX+GwIX5cucAN8AXc7wkvpArvYhFcrICaQGbEKZEasApmRf12B+ni5ji9QRxso93t/9M2nUfs52xXYFj7c6r5RuMCgYZ9+S3fIId9jq4eMX3AaPNcdslSFdwyjs7ma8D3f1nkNnu86dnszKaCNIlFPqJ8isCws/DC1hLkrsPavzzR15v9cq3tmEgVQbVlSXesDzRIrFFL5mXfNp+vcYQzD4IdvfiKXOauBYKcAAAAASUVORK5CYII=">
          </div>
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
import audio1 from "./assets/audio/Chaleya-Jawan-320-Kbps.mp3";
import audio2 from "./assets/audio/Hua-Main-Animal-320-Kbps.mp3";
import audio3 from "./assets/audio/Not-Ramaiya-Vastavaiya-Jawan-320-Kbps.mp3";

const title = ref("");
const currentIndex = ref(0);
const progress = ref(0);
const volume = ref(1);

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

const volumeUp = () => {
  if (volume.value < 1) {
    console.log(volume.value)
    volume.value = Number((volume.value + 0.1).toFixed(1));  
  }
};

const volumeDown = () => {
  if (volume.value > 0) {
    console.log(volume.value)
    volume.value = Number((volume.value - 0.1).toFixed(1));
  }
};

function setCurrentSong(index: number) {
  if(!currentSong.value.paused) currentSong.value.pause();
  currentIndex.value = index;
  currentSong.value.play()
}

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
