
<!-- svelte-ignore css_unused_selector -->
<script>
  let activeIndex = 0;
  const totalGroups = 3;
  let groups = [];

  // Set up a reference to each group
  import { onMount } from 'svelte';

  onMount(() => {
    groups = document.getElementsByClassName("card-group");
  });

  const handleLoveClick = () => {
    const nextIndex = activeIndex + 1 < totalGroups ? activeIndex + 1 : 0;

    // Set status of current and next group
    const currentGroup = groups[activeIndex];
    const nextGroup = groups[nextIndex];

    currentGroup.dataset.status = "after";
    nextGroup.dataset.status = "becoming-active-from-before";

    setTimeout(() => {
      nextGroup.dataset.status = "active";
      activeIndex = nextIndex;
    });
  };

  const handleHateClick = () => {
    const nextIndex = activeIndex - 1 >= 0 ? activeIndex - 1 : totalGroups - 1;

    // Set status of current and next group
    const currentGroup = groups[activeIndex];
    const nextGroup = groups[nextIndex];

    currentGroup.dataset.status = "before";
    nextGroup.dataset.status = "becoming-active-from-after";

    setTimeout(() => {
      nextGroup.dataset.status = "active";
      activeIndex = nextIndex;
    });
  };
</script>
<style>
    body {
  background-color: rgb(0, 0, 0);  
  margin: 0px;  
  height: 100vh;
  
  display: grid;
  place-items: center;
}

.card-groups,
.card-group,
.card {
  aspect-ratio: 5 / 7;
}

.card-groups,
.card-group,
.big-card {  
  width: 30vmin;
}

.card-group {
  position: absolute;
  transition: transform 400ms ease;
}

.card-group[data-status="unknown"] {
  transform: scale(0);
  transition: none;
}

.card-group[data-status="active"] {
  transition-delay: 300ms;
}

.card-group[data-status="after"] {
  transform: translateX(50%) scale(0);
}

.card-group[data-status="before"] {
  transform: translateX(-50%) scale(0);
}

.card-group[data-status="becoming-active-from-after"] {
  transform: translateX(50%) scale(0);
  transition: none;
}

.card-group[data-status="becoming-active-from-before"] {
  transform: translateX(-50%) scale(0);
  transition: none;
}

.card {
  background-color: rgba(255, 255, 255, 0.05);
  position: absolute;
  transition: transform 800ms cubic-bezier(.05,.43,.25,.95);
  
  background-position: center;
  background-size: cover;
}

.big-card {
  border-radius: 1vmin;
}

.little-card {
  width: 12vmin;
  border-radius: 2vmin;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: -1vmin 1vmin 2vmin rgba(0, 0, 0, 0.25);
  pointer-events: none;
}

.big-card:nth-child(2) {
  background-image: url("/e195c0df-e1f2-4c18-824d-e3b6a804ca6c.jfif");
  transform: translateX(-10%) rotate(-1deg);
}

.big-card:nth-child(4) {
  background-image: url("/2b58246a-069a-45fa-8303-46c15311908e.jfif");
  transform: rotate(2deg);
}

.big-card:nth-child(6) {
  background-image: url("/36d0f7d5-d795-43cb-8bb1-c1dd90c680c3.jfif");
  transform: translateX(-6%) rotate(-3deg);
}

.big-card:nth-child(8) {
  background-image: url("/3b41f45a-aa4d-4bd4-99ec-80e040d3418e.jfif");
  transform: translate(10%, 3%) rotate(5deg);
}

.card-group[data-index="1"] > .big-card:nth-child(6) {
  background-image: url("/b8243208-a127-4ce3-829c-6365a3a23c78.jfif");
}

.card-group[data-index="1"] > .big-card:nth-child(8) {
  background-image: url("/cba4ca2d-f72e-4516-9404-2d665552820d.jfif");
}

.card-group[data-index="2"] > .big-card:nth-child(4) {
  background-image: url("/00990603-30e2-43a8-840b-4cf83626a381.jfif");
}

.card-group[data-index="2"] > .big-card:nth-child(8) {
  background-image: url("/img1.jfif");
}

.little-card:nth-child(1) {
  background-image: url("/img2.jfif");
}

.little-card:nth-child(3) {
  background-image: url("/img3.jfif");
}

.little-card:nth-child(5) {
  background-image: url("/img4.jfif");
}

.little-card:nth-child(7) {
  background-image: url("/img5.jfif");
}

.card-group:hover > .card {  
  box-shadow: -2vmin 2vmin 3vmin rgba(0, 0, 0, 0.4);
}

.card-group:hover > .big-card:nth-child(2) {
  transform: translate(-75%, 16%) rotate(-24deg);
}

.card-group:hover > .big-card:nth-child(4) {
  transform: translate(-25%, 8%) rotate(-8deg);
}

.card-group:hover > .big-card:nth-child(6) {
  transform: translate(25%, 8%) rotate(8deg);
}

.card-group:hover > .big-card:nth-child(8) {
  transform: translate(75%, 16%) rotate(24deg);
}

.card-group:hover > .little-card:nth-child(1) {
  transform: translate(200%, -160%) rotate(-15deg);
}

.card-group:hover > .little-card:nth-child(3) {
  transform: translate(160%, 170%) rotate(15deg);
}

.card-group:hover > .little-card:nth-child(5) {
  transform: translate(-200%, -170%) rotate(15deg);
}

.card-group:hover > .little-card:nth-child(7) {
  transform: translate(-280%, 140%) rotate(-15deg);
}

.card-swiper-buttons {
  margin-top: 8vmin;
  display: flex;
  justify-content: space-around;  
  padding: 0vmin 4vmin;
}

.card-swiper-buttons > button {
  font-size: 2.5vmin;
  border: 0.4vmin solid rgb(200, 200, 200);
  border-radius: 100%;
  color: white;
  background-color: transparent;
  height: 10vmin;
  width: 10vmin;
  display: grid;
  place-items: center;
  cursor: pointer;
}

#love-button {
  color: rgb(33, 150, 243);
  border-color: rgb(33, 150, 243);
}

/* -- YouTube Link Styles -- */

body.menu-toggled > .meta-link > span {
  color: rgb(30, 30, 30);
}

#source-link {
  bottom: 60px;
}

#source-link > i {
  color: rgb(94, 106, 210);
}

#yt-link > i {
  color: rgb(239, 83, 80);
}

.meta-link {
  align-items: center;
  backdrop-filter: blur(3px);
  background-color: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 6px;
  bottom: 10px;
  box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.1);
  cursor: pointer;  
  display: inline-flex;
  gap: 5px;
  left: 10px;
  padding: 10px 20px;
  position: fixed;
  text-decoration: none;
  transition: background-color 400ms, border-color 400ms;
  z-index: 10000;
}

.meta-link:hover {
  background-color: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.meta-link > i, .meta-link > span {
  height: 20px;
  line-height: 20px;
}

.meta-link > span {
  color: white;
  font-family: "Rubik", sans-serif;
  font-weight: 500;
}
</style>
<div class="card-swiper flex justify-center items-center flex-col relative">
  <div class="card-groups">
    {#each [0, 1, 2] as groupIndex}
      <div class="card-group" 
           data-index={groupIndex} 
           bind:this={groups[groupIndex]}
           data-status={groupIndex === activeIndex ? "active" : "unknown"}>
        <div class="little-card card"></div>
        <div class="big-card card"></div>
        <div class="little-card card"></div>
        <div class="big-card card"></div>
        <div class="little-card card"></div>
        <div class="big-card card"></div>
        <div class="little-card card"></div>
        <div class="big-card card"></div>
      </div>
    {/each}
  </div>
  <div class="card-swiper-buttons gap-2">
    <button id="hate-button" on:click={handleHateClick}>
      👈
    </button>
    <button id="love-button" on:click={handleLoveClick}>
        👉
    </button>
  </div>
</div>