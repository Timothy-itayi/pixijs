<script>
// @ts-nocheck
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  const teams = [
    {
      id: 'mercedes',
      name: 'Mercedes',
      emblem: '/assets/teams/c.webp',
      drivers: [
        { id: 'antonelli', name: 'Antonelli', image: '/assets/drivers/antonelli.png' },
        { id: 'russell', name: 'Russell', image: '/assets/drivers/russell.avif' }
      ]
    },
    {
      id: 'redbull',
      name: 'Red Bull',
      emblem: '/assets/teams/redbull.jpeg',
      drivers: [
        { id: 'verstappen', name: 'Verstappen', image: '/assets/drivers/verstappen.avif' },
        { id: 'tsunoda', name: 'Tsunoda', image: '/assets/drivers/tsunoda.avif' }
      ]
    },
    {
      id: 'ferrari',
      name: 'Ferrari',
      emblem: '/assets/teams/ferrari.webp',
      drivers: [
        { id: 'leclerc', name: 'Leclerc', image: '/assets/drivers/leclerc.avif' },
        { id: 'hamilton', name: 'Hamilton', image: '/assets/drivers/hamilton.avif' }
      ]
    }
  ];

  let selectedTeam = null;
  let selectedDriver = null;

  function selectTeam(team) {
    if (selectedTeam?.id !== team.id) {
      selectedTeam = team;
      selectedDriver = null;
    }
  }

  function selectDriver(driver) {
    selectedDriver = driver;
  }

  function confirmSelection() {
    if (selectedTeam && selectedDriver) {
      dispatch('selectionConfirmed', {
        team: selectedTeam,
        driver: selectedDriver
      });
    }
  }
</script>

<div class="bg-black text-white min-h-screen flex flex-col items-center justify-center px-6 py-10">
  <h2 class="text-5xl font-light tracking-wide mb-16 border-b border-white pb-3 w-full max-w-4xl text-center">
    Select Your Team & Driver
  </h2>

  <!-- Team Cards -->
  <div class="flex space-x-12 mb-16 justify-center flex-wrap max-w-5xl">
    {#each teams as team}
      <div
        class="cursor-pointer border border-white rounded-lg p-8 flex flex-col items-center transition-transform hover:opacity-90 hover:scale-110"
        class:border-white={selectedTeam?.id === team.id}
        class:opacity-60={selectedTeam?.id !== team.id}
        on:click={() => selectTeam(team)}
        style="min-width: 200px;"
      >
        <img src={team.emblem} alt={team.name + ' emblem'} class="w-40 h-40 object-contain mb-6" />
        <p class="text-2xl tracking-wider">{team.name}</p>
      </div>
    {/each}
  </div>

  {#if selectedTeam}
    <h3 class="text-3xl font-light tracking-wide mb-8 border-b border-white pb-3 w-full max-w-4xl text-center">
      Drivers for {selectedTeam.name}
    </h3>
    <div class="flex space-x-12 justify-center flex-wrap max-w-5xl mb-12">
      {#each selectedTeam.drivers as driver}
        <div
          class="cursor-pointer border border-white rounded-lg p-6 flex flex-col items-center transition-transform hover:scale-110"
          class:border-white={selectedDriver?.id === driver.id}
          class:opacity-60={selectedDriver?.id !== driver.id}
          on:click={() => selectDriver(driver)}
          style="min-width: 160px;"
        >
          <img src={driver.image} alt={driver.name} class="w-32 h-32 object-cover rounded-full mb-4" />
          <p class="text-lg tracking-wide">{driver.name}</p>
        </div>
      {/each}
    </div>
  {/if}

  <button
    on:click={confirmSelection}
    disabled={!selectedTeam || !selectedDriver}
    class="mt-6 px-12 py-4 border border-white rounded-lg uppercase tracking-widest hover:bg-white hover:text-black transition-colors duration-200 disabled:opacity-30"
  >
    Confirm Selection
  </button>
</div>
