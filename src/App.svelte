<script lang="ts">
  import ChatBot from "./lib/ChatBot.svelte";

  import Navbar from "./lib/Navbar.svelte";
  import img from "./assets/grid.svg";
  import Hero from "./lib/Hero.svelte";
  import QuickfindGrid from "./lib/QuickfindGrid.svelte";
</script>

<main>
  <Navbar />
  <img src={img} alt="grid" class="bg-grid" />
  <Hero />
  <QuickfindGrid />
  <ChatBot />
</main>

<style>
  main {
    position: relative;
    padding: 0 2rem;
  }

  .bg-grid {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    mask-image: linear-gradient(180deg, rgba(0, 0, 0, 1) 0%, rgba(2, 0, 36, 0) 35%);
  }
</style>
