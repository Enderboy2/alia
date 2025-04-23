
<script>
  import { onMount, tick } from 'svelte';
  import Card from '../components/card.svelte';
  import RewardModal from '../components/rewardModal.svelte';
  import ConfettiEffect from '../components/confettiEffect.svelte';
  
  // Game state
  let cards = [];
  let flippedCards = [];
  let matchedPairs = 0;
  let totalPairs = 8;
  let gameStarted = false;
  let gameCompleted = false;
  let showReward = false;
  let currentReward = null;
  let moves = 0;
  let showConfetti = false;
  
  // Timer state
  let seconds = 0;
  let timerInterval;
  
  // Start the timer
  function startTimer() {
    seconds = 0;
    clearInterval(timerInterval);
    timerInterval = setInterval(() => {
      seconds++;
    }, 1000);
  }
  
  // Stop the timer
  function stopTimer() {
    clearInterval(timerInterval);
  }
  
  // Format time as MM:SS
  function formatTime(totalSeconds) {
    const minutes = Math.floor(totalSeconds / 60);
    const secs = totalSeconds % 60;
    return `${minutes.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  }
  
  // Rewards array - can be customized later
  const rewards = [
    { type: 'coupon', title: 'Sweet tooth', description: 'Redeem for a special snack of your choice(kinder wla flutes y3ni?)' },
    { type: 'memory', title: 'Remember When...', description: 'Our first date?, best ice cream suki have ever tried (hint:20/6/2024)' },
    { type: 'song', title: 'LAMA BETETGANENNNN', description: 'First song i shared with you (long time ago 3mtn)' },
    { type: 'coupon', title: 'Spawn Point', description: 'use this carefully, spawn your idiot bf anytime anywhere immediatly(relaisticly between 30M-1H)' },
    { type: 'memory', title: 'First valentine!!', description: 'Actually a first for me' },
    { type: 'memory', title: 'NEW INTELLLL', description: '23/3/2025 was my fav date tbh do you agree?' },
    { type: 'memory', title: 'That was funn', description: '9/1/2025 we made these (makowka picrew)s of eachothers, i relly enjoyed it!!! lets do it again soon' },
    { type: 'memory', title: 'Meant to be', description: 'the first picture of us alone was on 22/2/2024, fakraha?' },
  ];
  
  // Generate the card deck
  function generateCards() {
    const cardTypes = [
      'heart', 'star', 'moon', 'sun', 
      'cloud', 'flower', 'tree', 'bird'
    ];
    
    let tempCards = [];
    cardTypes.forEach((type, index) => {
      // Create pair of cards
      tempCards.push({ id: index * 2, type, flipped: false, matched: false });
      tempCards.push({ id: index * 2 + 1, type, flipped: false, matched: false });
    });
    
    // Shuffle cards
    cards = shuffleArray(tempCards);
    matchedPairs = 0;
    moves = 0;
    gameCompleted = false;
    flippedCards = [];
  }
  
  // Fisher-Yates shuffle algorithm for randomizing cards
  function shuffleArray(array) {
    let currentIndex = array.length;
    let temporaryValue, randomIndex;
    
    while (currentIndex !== 0) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex -= 1;
      
      temporaryValue = array[currentIndex];
      array[currentIndex] = array[randomIndex];
      array[randomIndex] = temporaryValue;
    }
    
    return array;
  }
  
  // Handle card flipping logic
  async function handleCardFlip(id) {
    // Don't allow flips if already flipped or matched
    const cardToFlip = cards.find(card => card.id === id);
    if (cardToFlip.flipped || cardToFlip.matched || flippedCards.length >= 2) {
      return;
    }
    
    // Flip the card
    cards = cards.map(card => {
      if (card.id === id) {
        return { ...card, flipped: true };
      }
      return card;
    });
    
    flippedCards = [...flippedCards, cardToFlip];
    
    // Check for match if we have 2 cards flipped
    if (flippedCards.length === 2) {
      moves++;
      
      if (flippedCards[0].type === flippedCards[1].type) {
        // Match found
        await tick();
        setTimeout(() => {
          cards = cards.map(card => {
            if (card.id === flippedCards[0].id || card.id === flippedCards[1].id) {
              return { ...card, matched: true };
            }
            return card;
          });
          
          matchedPairs++;
          flippedCards = [];
          
          // Check for game completion
          if (matchedPairs === totalPairs) {
            completeGame();
          }
        }, 500);
      } else {
        // No match
        await tick();
        setTimeout(() => {
          cards = cards.map(card => {
            if (card.id === flippedCards[0].id || card.id === flippedCards[1].id) {
              return { ...card, flipped: false };
            }
            return card;
          });
          
          flippedCards = [];
        }, 1000);
      }
    }
  }
  
  // Game state
  let canPlay = true;
  let nextPlayTime = null;
  
  // Check if user can play
  function checkPlayEligibility() {
    const lastPlayTime = localStorage.getItem('lastPlayTime');
    if (lastPlayTime) {
      const timeSinceLastPlay = Date.now() - parseInt(lastPlayTime);
      const hoursRemaining = 24 - (timeSinceLastPlay / (1000 * 60 * 60));
      
      if (hoursRemaining > 0) {
        canPlay = false;
        nextPlayTime = new Date(parseInt(lastPlayTime) + (24 * 60 * 60 * 1000));
        return false;
      }
    }
    return true;
  }
  
  // Format time until next play
  function formatTimeUntilNext(nextTime) {
    const now = new Date();
    const diff = nextTime - now;
    const hours = Math.floor(diff / (1000 * 60 * 60));
    const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    return `${hours}h ${minutes}m`;
  }
  
  // Start a new game
  function startNewGame() {
    if (!checkPlayEligibility()) return;
    
    showReward = false;
    showConfetti = false;
    seconds = 0;
    generateCards();
    gameStarted = true;
    startTimer();
    localStorage.setItem('lastPlayTime', Date.now().toString());
  }
  
  // Game completion actions
  function completeGame() {
    gameCompleted = true;
    stopTimer();
    
    const randomIndex = Math.floor(Math.random() * rewards.length);
    currentReward = rewards[randomIndex];
    
    showConfetti = true;
    setTimeout(() => {
      showReward = true;
    }, 1500);
  }
  
  // Close reward modal and restart
  function closeReward() {
    showReward = false;
    showConfetti = false;
    gameStarted = false;
    gameCompleted = false;
  }
  
  // Move the global reset function inside onMount
  onMount(() => {
    // Add global reset function
    window.yoma = () => {
      localStorage.removeItem('lastPlayTime');
      canPlay = true;
      nextPlayTime = null;
    };

    generateCards();
    checkPlayEligibility();
    return () => {
      stopTimer();
    };
  });
</script>

<main class="min-h-screen bg-gradient-to-br from-sky-400 via-blue-500 to-indigo-600 flex flex-col items-center p-2 sm:p-4">
  <header class="text-center mb-2 sm:mb-4 mt-2 sm:mt-4">

    
    {#if !gameStarted}
      <div class="mt-4 sm:mt-8 max-w-md mx-auto bg-white/10 backdrop-blur-sm p-4 sm:p-6 rounded-lg shadow-2xl">
        <p class="text-white text-base font-semibold sm:text-lg mb-4 sm:mb-6">Made especially for suuuuu</p>
        {#if canPlay}
          <button 
            on:click={startNewGame}
            class="bg-transparent text-white px-6 sm:px-8 py-2 sm:py-3 rounded-md font-bold shadow-lg border-2 border-white/70 hover:bg-white hover:text-blue-600 transition-all duration-300"
          >
            Start Game
          </button>
        {:else}
          <div class="text-white text-center">
            <p class="mb-2">You can play again in:</p>
            <p class="text-2xl font-bold">{formatTimeUntilNext(nextPlayTime)}</p>
          </div>
        {/if}
      </div>
    {:else}
      <div class="flex items-center justify-center gap-3 sm:gap-4 mb-2 sm:mb-4">
        <div class="bg-white/20 backdrop-blur-sm px-3 sm:px-5 py-1 sm:py-2 rounded-md shadow-lg border border-white/30">
          <div class="flex items-center">
            <div class="bg-blue-700/60 rounded-md p-1 mr-2">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 sm:h-5 sm:w-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
            </div>
            <p class="text-white text-sm sm:text-base font-medium">Moves: <span class="font-bold">{moves}</span></p>
          </div>
        </div>
      </div>
    {/if}
  </header>
  
  {#if gameStarted}
    <div class="grid grid-cols-4 gap-2 sm:gap-3 md:gap-4 w-full max-w-[90vh] mx-auto">
      {#each cards as card (card.id)}
        <div class="w-full aspect-square">
          <Card 
            {card} 
            onFlip={() => handleCardFlip(card.id)}
          />
        </div>
      {/each}
    </div>
    
    <button 
      on:click={startNewGame}
      class="mt-4 sm:mt-6 bg-transparent text-white px-4 sm:px-6 py-1 sm:py-2 rounded-md text-sm sm:text-base font-medium shadow-lg border-2 border-white/70 hover:bg-white hover:text-blue-600 transition-all duration-300"
    >
      New Game
    </button>

    <!-- Add this button for testing -->
    
  {/if}
  
  {#if showConfetti}
    <ConfettiEffect />
  {/if}
  
  {#if showReward}
    <RewardModal reward={currentReward} onClose={closeReward} />
  {/if}
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: 'Poppins', 'Arial', sans-serif;
    overflow-x: hidden;
    background-color: #4338ca; /* Fallback color */
  }
  
  .font-arabic {
    font-family: 'Amiri', 'Scheherazade New', serif;
  }
  
  @keyframes float {
    0% { transform: translateY(0px); }
    50% { transform: translateY(-10px); }
    100% { transform: translateY(0px); }
  }
  
  :global(.floating) {
    animation: float 3s ease-in-out infinite;
  }
</style>