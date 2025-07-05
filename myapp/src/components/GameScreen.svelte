<script>
  let raceStarted = false;
  let lap = 0;
  let sector = 0;

  const drivers = [
    { name: 'Mia Vargas', position: 1, tyres: 'Soft' },
    { name: 'Theo Blake', position: 2, tyres: 'Medium' },
    { name: 'Lucas Rey', position: 3, tyres: 'Hard' },
    { name: 'Anna Ford', position: 4, tyres: 'Soft' }
  ];

  const eventCards = [
    { id: 1, title: 'Attack Corner', description: 'Attempt to overtake in corner' },
    { id: 2, title: 'Defend Position', description: 'Hold your current position' },
    { id: 3, title: 'Push Hard', description: 'Increase pace but risk tyres' }
  ];

  function startRace() {
    raceStarted = true;
    lap = 1;
    sector = 1;
  }

  function nextSector() {
    sector++;
    if (sector > 3) {
      sector = 1;
      lap++;
    }
  }

  // @ts-ignore
  function playEventCard(card) {
    alert(`Played card: ${card.title} — ${card.description}`);
  }
</script>

<div class="min-h-screen bg-black text-white flex flex-col lg:flex-row gap-6 p-6">
  <!-- Track Map Section -->
  <div class="flex-1 bg-neutral-900 rounded-lg flex items-center justify-center p-4">
    <img src="/assets/tracks/MonacoGP.png" alt="Race Track" class="max-w-full max-h-full rounded-md" />
  </div>

  <!-- Info / Control Section -->
  <div class="flex-1 flex flex-col gap-6">
    
    <!-- Race Info -->
    <div class="bg-neutral-900 p-6 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-4">Race Info</h3>
      <p class="text-lg mb-4">Lap: {lap} &nbsp;&nbsp; Sector: {sector}</p>
      <button
        on:click={nextSector}
        class="px-6 py-2 border border-white rounded-md uppercase tracking-wide hover:bg-white hover:text-black transition"
        disabled={!raceStarted}
      >
        Next Sector
      </button>
    </div>

    <!-- Grid Order -->
    <div class="bg-neutral-900 p-6 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-4">Grid Order</h3>
      {#each drivers as driver}
        <div class="flex justify-between py-2 border-b border-neutral-700 text-sm">
          <span>#{driver.position} {driver.name}</span>
          <span>Tyres: {driver.tyres}</span>
        </div>
      {/each}
    </div>

    <!-- Event Cards -->
    <div class="bg-neutral-900 p-6 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-4">Your Event Cards</h3>
      <div class="flex flex-wrap gap-4">
        {#each eventCards as card}
          <div
            on:click={() => playEventCard(card)}
            class="border border-white px-4 py-3 rounded-md cursor-pointer hover:bg-white hover:text-black transition text-sm font-medium"
          >
            {card.title}
          </div>
        {/each}
      </div>
    </div>

    <!-- Race Director -->
    <div class="bg-neutral-900 p-6 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-4">Race Director</h3>
      <button
        on:click={startRace}
        disabled={raceStarted}
        class="px-6 py-2 border border-white rounded-md uppercase tracking-wide hover:bg-white hover:text-black transition disabled:opacity-30"
      >
        {raceStarted ? 'Race Started' : 'Start Race'}
      </button>
    </div>

  </div>
</div>
