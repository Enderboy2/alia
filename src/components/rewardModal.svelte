<script>
  export let reward;
  export let onClose;
</script>

<div class="fixed inset-0 flex items-center justify-center z-50 px-4 animate-fadeIn">
  <div class="absolute inset-0 bg-black/60 backdrop-blur-sm animate-fadeIn" on:click={onClose}></div>
  
  <div class="relative bg-white rounded-2xl shadow-2xl p-6 md:p-8 max-w-md w-full animate-slideUp">
    <button 
      on:click={onClose}
      class="absolute top-3 right-3 text-gray-500 hover:text-gray-800 transition-colors hover:rotate-90 transform duration-300"
    >
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
      </svg>
    </button>
    
    <div class="text-center">
      <div class="mb-4 transform hover:scale-110 transition-transform duration-300">
        {#if reward.type === 'coupon'}
          <div class="inline-block p-3 bg-gradient-to-br from-sky-500 to-blue-600 rounded-full hover:shadow-xl transition-all duration-300">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7" />
            </svg>
          </div>
        {:else if reward.type === 'memory'}
          <div class="inline-block p-3 bg-gradient-to-br from-sky-500 to-blue-600 rounded-full hover:shadow-xl transition-all duration-300">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" />
            </svg>
          </div>
        {:else if reward.type === 'song'}
          <div class="inline-block p-3 bg-gradient-to-br from-sky-500 to-blue-600 rounded-full hover:shadow-xl transition-all duration-300">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3" />
            </svg>
          </div>
        {/if}
      </div>
      
      <h2 class="text-2xl font-bold text-gray-800 mb-2 hover:text-blue-600 transition-colors duration-300">{reward.title}</h2>
      <p class="text-gray-600 mb-6 hover:text-gray-800 transition-colors duration-300">{reward.description}</p>
      
      <div class="border-t border-gray-200 pt-6">
        <button 
          on:click={onClose}
          class="bg-gradient-to-r from-sky-500 to-blue-600 text-white px-6 py-3 rounded-md font-medium shadow-md hover:shadow-xl transform hover:scale-105 transition-all duration-300 hover:from-blue-600 hover:to-sky-500"
        >
          Claim
        </button>
      </div>
    </div>
  </div>
</div>

<style>
  .animate-fadeIn {
    animation: fadeIn 0.3s ease-out;
  }
  
  .animate-slideUp {
    animation: slideUp 0.4s ease-out;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  
  @keyframes slideUp {
    from { 
      opacity: 0;
      transform: translateY(20px);
    }
    to { 
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
