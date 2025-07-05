<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  function startGame() {
    dispatch('start');
  }
</script>

<div class="flex flex-col items-center justify-center h-screen bg-black text-white px-4">
  <h1 class="text-5xl tracking-wide font-light border-b border-white pb-2 mb-12">
    Drive to Survive
  </h1>

  <button
    on:click={startGame}
    class="px-8 py-3 border border-white rounded-md text-white tracking-wide uppercase hover:bg-white hover:text-black transition-colors duration-200"
  >
    Enter Paddock
  </button>
</div>
