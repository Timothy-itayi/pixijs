<script>
// @ts-nocheck
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  const teams = [
    {
      id: 'mercedes',
      name: 'Mercedes',
      emblem: '/assets/teams/mercedes.png',
      drivers: [
        { id: 'hamilton', name: 'Hamilton', image: '/assets/drivers/hamilton.png' },
        { id: 'russell', name: 'Russell', image: '/assets/drivers/russell.png' }
      ]
    },
    {
      id: 'redbull',
      name: 'Red Bull',
      emblem: '/assets/teams/redbull.png',
      drivers: [
        { id: 'verstappen', name: 'Verstappen', image: '/assets/drivers/verstappen.png' },
        { id: 'perez', name: 'Perez', image: '/assets/drivers/perez.png' }
      ]
    },
    {
      id: 'ferrari',
      name: 'Ferrari',
      emblem: '/assets/teams/ferrari.png',
      drivers: [
        { id: 'leclerc', name: 'Leclerc', image: '/assets/drivers/leclerc.png' },
        { id: 'sainz', name: 'Sainz', image: '/assets/drivers/sainz.png' }
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

<div class="px-6 py-10 bg-black text-white min-h-full flex flex-col items-center">
  <h2 class="text-4xl font-light tracking-wide mb-10 border-b border-white pb-2">
    Select Your Team & Driver
  </h2>

  <!-- Team Cards -->
  <div class="flex space-x-6 mb-10 overflow-x-auto pb-2">
    {#each teams as team}
      <div
        class="cursor-pointer border border-white rounded-md p-4 flex flex-col items-center transition-all hover:opacity-90 hover:scale-105"
        class:border-white={selectedTeam?.id === team.id}
        class:opacity-60={selectedTeam?.id !== team.id}
        on:click={() => selectTeam(team)}
        style="min-width: 150px;"
      >
        <img src={team.emblem} alt={team.name + ' emblem'} class="w-24 h-24 object-contain mb-3" />
        <p class="text-lg tracking-wider">{team.name}</p>
      </div>
    {/each}
  </div>

  {#if selectedTeam}
    <h3 class="text-2xl font-light tracking-wide mb-6 border-b border-white pb-2">
      Drivers for {selectedTeam.name}
    </h3>
    <div class="flex space-x-6 overflow-x-auto pb-6 mb-6">
      {#each selectedTeam.drivers as driver}
        <div
          class="cursor-pointer border border-white rounded-md p-3 flex flex-col items-center transition-all hover:scale-105"
          class:border-white={selectedDriver?.id === driver.id}
          class:opacity-60={selectedDriver?.id !== driver.id}
          on:click={() => selectDriver(driver)}
          style="min-width: 120px;"
        >
          <img src={driver.image} alt={driver.name} class="w-20 h-20 object-cover rounded-full mb-2" />
          <p class="text-sm tracking-wide">{driver.name}</p>
        </div>
      {/each}
    </div>
  {/if}

  <button
    on:click={confirmSelection}
    disabled={!selectedTeam || !selectedDriver}
    class="mt-6 px-8 py-3 border border-white rounded-md uppercase tracking-widest hover:bg-white hover:text-black transition-colors duration-200 disabled:opacity-30"
  >
    Confirm Selection
  </button>
</div>
