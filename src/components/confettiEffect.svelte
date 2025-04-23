// components/ConfettiEffect.svelte
<script>
  import { onMount, onDestroy } from 'svelte';
  
  let canvas;
  let context;
  let confetti = [];
  let animationFrameId;
  
  const colors = ['#3B82F6', '#93C5FD', '#60A5FA', '#1E40AF', '#FFFFFF'];
  
  // Create confetti piece
  function createConfetti() {
    return {
      x: Math.random() * window.innerWidth,
      y: -20,
      size: Math.random() * 10 + 5,
      color: colors[Math.floor(Math.random() * colors.length)],
      rotation: Math.random() * 360,
      rotationSpeed: Math.random() * 5 - 2.5,
      speed: Math.random() * 3 + 2,
      opacity: 1
    };
  }
  
  // Draw confetti pieces
  function drawConfetti() {
    context.clearRect(0, 0, canvas.width, canvas.height);
    
    confetti.forEach((piece, index) => {
      context.save();
      context.translate(piece.x, piece.y);
      context.rotate((piece.rotation * Math.PI) / 180);
      context.globalAlpha = piece.opacity;
      context.fillStyle = piece.color;
      
      // Draw star shape for confetti
      if (Math.random() > 0.5) {
        context.fillRect(-piece.size / 2, -piece.size / 2, piece.size, piece.size);
      } else {
        context.beginPath();
        context.arc(0, 0, piece.size / 2, 0, Math.PI * 2);
        context.fill();
      }
      
      context.restore();
      
      // Update confetti piece
      piece.y += piece.speed;
      piece.rotation += piece.rotationSpeed;
      
      // Remove confetti when it's off screen or faded
      if (piece.y > canvas.height) {
        piece.opacity -= 0.05;
        if (piece.opacity <= 0) {
          confetti.splice(index, 1);
        }
      }
    });
    
    // Add new confetti
    if (confetti.length < 100) {
      confetti.push(createConfetti());
    }
    
    animationFrameId = requestAnimationFrame(drawConfetti);
  }
  
  // Initialize canvas on mount
  onMount(() => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    context = canvas.getContext('2d');
    
    // Generate initial confetti
    for (let i = 0; i < 50; i++) {
      confetti.push(createConfetti());
    }
    
    // Start animation
    drawConfetti();
    
    // Handle window resize
    const handleResize = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    };
    
    window.addEventListener('resize', handleResize);
    
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  });
  
  // Clean up animation on component destroy
  onDestroy(() => {
    if (animationFrameId) {
      cancelAnimationFrame(animationFrameId);
    }
  });
</script>

<canvas 
  bind:this={canvas} 
  class="fixed inset-0 pointer-events-none z-40"
></canvas>
