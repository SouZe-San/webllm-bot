<script lang="ts">
  import QuickfindCard from "./QuickfindCard.svelte";

  const quickfindItems = [
    {
      title: "What is Our Platform?",
      description: "Here for the first time? Learn how our platform can help you grow.",
      icon: "M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z",
    },
    {
      title: "The Platform Features",
      description: "Tracking your customers in the unified platform for growth.",
      icon: "M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10",
    },
    {
      title: "Installing Platform",
      description: "Everything you need to know to install and set up your workspace.",
      icon: "M8 4H6a2 2 0 00-2 2v12a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2h-2m-4-1v8m0 0l3-3m-3 3L9 8m-5 5h2.586a1 1 0 01.707.293l2.414 2.414a1 1 0 00.707.293h3.172a1 1 0 00.707-.293l2.414-2.414a1 1 0 01.707-.293H20",
    },
    {
      title: "Getting started 101",
      description: "Everything you need to know to get started with our platform.",
      icon: "M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253",
    },
  ];
</script>

<section>
  <h2>Quickfind answers</h2>
  <div class="quickfind-flex">
    {#each quickfindItems as item}
      <QuickfindCard {...item} />
    {/each}
  </div>
</section>

<style>
  section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
  }

  h2 {
    margin: 3rem 0 2rem;
  }
</style>
