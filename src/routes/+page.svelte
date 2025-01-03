<script>
	import Chat from "./Chat.svelte";
	import Hero from "./hero.svelte";
	import Pictures from "./Pictures.svelte";
	import StaryText from "./staryText.svelte";

  // No dynamic logic, just plain HTML for sections.
</script>

<style>
  .snap-container {
    scroll-snap-type: y mandatory;
    overflow-y: scroll;
    height: 100vh;
    scroll-behavior: smooth;
  }

  .snap-section {
    scroll-snap-align: start;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    padding: 2rem;
    color: white;
  }


</style>
<div class="snap-container">
  <section class="snap-section section1 flex flex-col items-center justify-center bg-black">
    <StaryText></StaryText>
  </section>
  
  <section class="snap-section bg-black flex-col">
    <h1 class="mb-12">idk, but here is some pictures tho</h1>
    <Pictures />
    <p class="text-sm mt-10">*the buttons just change the layout*</p> 
  </section>
  
  <section class="snap-section bg-black ">
    <Hero />
  </section>
  
  <section class="snap-section bg-black overflow-hidden">
        <Chat />
  </section>
</div>
