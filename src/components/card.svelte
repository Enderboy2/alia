<script>
  export let card;
  export let onFlip;
</script>

<div 
  class="aspect-square perspective-1000 cursor-pointer transform hover:scale-105 transition-transform duration-200 w-full h-full"
  on:click={onFlip}
>
  <div class={`relative w-full h-full transition-transform duration-500 transform-style-3d ${card.flipped ? 'rotate-y-180' : ''}`}>
    <!-- Card Back -->
    <div class={`absolute w-full h-full backface-hidden rounded-xl shadow-lg ${card.matched ? 'opacity-70' : ''}`}>
      <div class="w-full h-full bg-white rounded-xl flex items-center justify-center p-2">
        <div class="text-sky-500 text-4xl font-bold">?</div>
      </div>
    </div>
    
    <!-- Card Front -->
    <div class="absolute w-full h-full backface-hidden rotate-y-180 rounded-xl shadow-lg overflow-hidden">
      <div class="w-full h-full bg-white flex items-center justify-center p-2">
        <div class="text-5xl">
          {#if card.type === 'heart'}
            <span>❤️</span>
          {:else if card.type === 'star'}
            <span>⭐</span>
          {:else if card.type === 'moon'}
            <span>🌙</span>
          {:else if card.type === 'sun'}
            <span>☀️</span>
          {:else if card.type === 'cloud'}
            <span>☁️</span>
          {:else if card.type === 'flower'}
            <span>🌸</span>
          {:else if card.type === 'tree'}
            <span>🌳</span>
          {:else if card.type === 'bird'}
            <span>🐦</span>
          {/if}
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .perspective-1000 {
    perspective: 1000px;
  }
  
  .transform-style-3d {
    transform-style: preserve-3d;
  }
  
  .backface-hidden {
    backface-visibility: hidden;
  }
  
  .rotate-y-180 {
    transform: rotateY(180deg);
  }
</style>