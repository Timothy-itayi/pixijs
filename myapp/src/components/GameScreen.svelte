<script>
// @ts-nocheck
 import ActionCard from './ActionCard.svelte';
  let raceStarted = false;
  let lap = 0;
  let sector = 0;
  let showTyreAge = false;

  const drivers = Array.from({ length: 20 }, (_, i) => ({
    name: `Driver ${i + 1}`,
    position: i + 1,
    tyres: ['Soft', 'Medium', 'Hard'][i % 3],
    tyreAge: 10 + i
  }));

 const actionCards = [
    {
      title: 'Attack Corner',
      description: 'Overtake attempt in a corner',
      imageUrl: '/assets/action_cards/over.jpg',
    },
    {
      title: 'Defend Position',
      description: 'Block opponent advances',
      imageUrl: '/assets/action_cards/de.jpg',
    },
    {
      title: 'Push Hard',
      description: 'Boost pace, higher tyre wear',
      imageUrl: '/assets/action_cards/push.jpg',
    },
  ];

  function playActionCard(card) {
    eventFeed.unshift(`🎴 Played: ${card.title} — ${card.description}`);
  }
  // @ts-ignore
  const eventFeed = [];
  let radioMessage = 'Welcome to MonacoGP! Your engineer will guide you.';

  function startRace() {
    raceStarted = true;
    lap = 1;
    sector = 1;
    eventFeed.unshift(`🏁 Lap ${lap}, Sector ${sector} begins.`);
  }

  function nextSector() {
    if (!raceStarted) return;
    sector++;
    if (sector > 3) {
      sector = 1;
      lap++;
    }
    eventFeed.unshift(`📍 Sector ${sector} of Lap ${lap}...`);
  }

  // @ts-ignore

  // @ts-ignore
  function respondToRadio(response) {
    eventFeed.unshift(`🗣️ You: ${response}`);
    radioMessage = 'Pit confirmed. Box this lap.';
  }
</script>

<style>
  .scrollbar-thin::-webkit-scrollbar {
    width: 4px;
  }
  .scrollbar-thin::-webkit-scrollbar-thumb {
    background-color: rgba(255, 255, 255, 0.3);
    border-radius: 2px;
  }
</style>

<div class="min-h-screen bg-black text-white flex flex-col lg:flex-row gap-4 p-4 border border-red-600">

  <!-- LEFT PANEL: Game World -->
  <div class="w-full lg:w-3/5 flex flex-col gap-4">

    <!-- Race Feed -->
    <div class="bg-neutral-900 p-4 rounded-lg max-h-40 overflow-y-auto scrollbar-thin">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-2">📢 Race Feed</h3>
      <ul class="space-y-1 text-sm">
        {#each eventFeed as event, i}
          <li key={i} class="opacity-80">{event}</li>
        {/each}
      </ul>
    </div>

    <!-- Track Map + Grid Order -->
    <div class="flex gap-4">
      <!-- Track Map -->
      <div class="bg-neutral-900 rounded-lg p-2 h-[240px] w-4/5 relative">
        <div class="absolute top-2 left-2 bg-black/60 p-2 rounded text-xs border border-white">
          Lap {lap} / 58<br />Sector {sector} / 3
        </div>
        <img src="/assets/tracks/MonacoGP.png" alt="Monaco Map" class="w-full h-full object-contain rounded-md" />
      </div>

      <!-- Grid Order -->
      <div class="w-1/5 bg-neutral-900 p-3 rounded-lg overflow-y-auto max-h-[240px] scrollbar-thin text-xs">
        <div class="flex justify-between items-center mb-2">
          <h3 class="font-light">Grid</h3>
          <button
            class="text-[10px] border px-2 py-1 rounded hover:bg-white hover:text-black transition"
            on:click={() => (showTyreAge = !showTyreAge)}
          >
            {showTyreAge ? 'Tyre Type' : 'Tyre Age'}
          </button>
        </div>
        <ul class="space-y-1">
          {#each drivers as d}
            <li class="border-b border-neutral-700 pb-1">
              <strong>#{d.position} {d.name}</strong><br />
              {showTyreAge
                ? `Tyre: ${d.tyres} - ${d.tyreAge} laps`
                : `Tyres: ${d.tyres}`}
            </li>
          {/each}
        </ul>
      </div>
    </div>
  </div>

  <!-- RIGHT PANEL: Player Interface -->
  <div class="w-full lg:w-2/5 flex flex-col gap-4">

    <!-- Radio Message Box -->
    <div class="bg-neutral-900 p-4 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-3">🎧 Engineer Radio</h3>
      <p class="text-sm mb-3">{radioMessage}</p>
      <div class="flex flex-wrap gap-2 text-xs">
        <button on:click={() => respondToRadio('Should we pit?')} class="border px-3 py-1 rounded hover:bg-white hover:text-black">Ask for pit strategy</button>
        <button on:click={() => respondToRadio('What’s the weather update?')} class="border px-3 py-1 rounded hover:bg-white hover:text-black">Weather status</button>
        <button on:click={() => respondToRadio('Push or conserve?')} class="border px-3 py-1 rounded hover:bg-white hover:text-black">Pace advice</button>
      </div>
    </div>

    <!-- Driver Stats -->
    <div class="bg-neutral-900 p-4 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-3">🏎️ Driver Stats</h3>
      <p class="text-sm">Tyres: Medium</p>
      <p class="text-sm">Tyre Wear: 18%</p>
      <p class="text-sm">Fuel: 60%</p>
      <p class="text-sm">Current Pace: Balanced</p>
    </div>

    <!-- Action Cards -->
  <!-- Action Cards -->
<div class="bg-neutral-900 p-4 rounded-lg">
  <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-3">🎴 Action Cards</h3>
  <div class="flex flex-wrap gap-3">
    {#each actionCards as card}
      <ActionCard
        title={card.title}
        description={card.description}
        imageUrl={card.imageUrl}
        on:click={() => playActionCard(card)}
      />
    {/each}
  </div>
</div>


    <!-- Race Director -->
    <div class="bg-neutral-900 p-4 rounded-lg">
      <h3 class="text-xl font-light tracking-wide border-b border-white pb-2 mb-3">📋 Race Director</h3>
      <div class="flex gap-2">
        <button
          on:click={startRace}
          disabled={raceStarted}
          class="px-4 py-2 border rounded-md uppercase tracking-wide hover:bg-white hover:text-black transition disabled:opacity-40"
        >
          {raceStarted ? 'Race Started' : 'Start Race'}
        </button>
        <button
          on:click={nextSector}
          disabled={!raceStarted}
          class="px-4 py-2 border rounded-md uppercase tracking-wide hover:bg-white hover:text-black transition disabled:opacity-40"
        >
          Next Sector
        </button>
      </div>
    </div>
  </div>
</div>
