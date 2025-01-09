<script lang="ts">
  export let title: string;
  export let description: string;
  export let icon: string;
</script>

<div class="quickfind-card">
  <div class="card-icon">
    <svg width="24" height="24" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={icon}></path>
    </svg>
  </div>
  <h3 class="card-title">{title}</h3>
  <p class="card-description">{description}</p>
</div>