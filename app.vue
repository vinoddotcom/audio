<template>
<div class="player">

<div class="header">
  Music <span>Player</span>
</div>

<div class="img">
  <img src="" id="thumb">
</div>
<div class="details">
  <h3 id="title"></h3>
  <p id="musician"></p>
</div>
<audio id="song">
  <source src="" type="audio/mp3">
  </source>
</audio>

<div class="time">
  <span id="start">2:28</span>
  <span id="end">4:33</span>
</div>
<input type="range" id="progress" value="0">

<div class="action">

  <button onclick="prevPlay()">
    <i class="bx bx-skip-previous"></i>
  </button>

  <button onclick="playPause()" class="play" id="play">
    <i class="bx bx-play"></i>
  </button>

  <button onclick="nextPlay()">
    <i class="bx bx-skip-next"></i>
  </button>

</div>
</div>
</template>
<script setup lang="ts">

</script>