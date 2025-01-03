<script>
  import { onMount } from "svelte";
  import { fade, scale } from 'svelte/transition';

  let words = ["A boosted valo player", "Best wingwoman", "A disocrd nerd", "unnecessarily shy girl", "a girl who's fun to be around", "A FALA7A!!!"];
  let currentWordIndex = 0;
  let word = words[currentWordIndex];

  onMount(() => {
    const interval = setInterval(() => {
      currentWordIndex = (currentWordIndex + 1) % words.length;
      word = words[currentWordIndex];
    }, 3000); // Change every 3 seconds

    return () => clearInterval(interval);
  });

  // Custom transition combining fade and scale
  function combinedTransition(node, params) {
    return {
      duration: 300,
      css: (t) => {
        const fadeValue = Math.pow(t, 3);
        const scaleValue = 1 + (1 - t) * 0.1; // Scale up by 10% on entry
        return `
          opacity: ${fadeValue};
          
        `;
      }
    };
  }
</script>

<div class="relative ">
    <h1 class="block">Actually, just remembered that she is</h1>
  <h1 class="text-lg">
    <span class="inline-block">
      {#key word}
        <h1 transition:combinedTransition class="h-fit w-fit text-4xl text-wrap font-bold text-red-800 ">
          {word}
        </h1>
      {/key}
    </span>
  </h1>
</div>

<style>
  /* Additional Tailwind or custom styles can go here */
</style>
