<script>
// @ts-nocheck

  import TitleScreen from '../components/TitleScreen.svelte';
  import TeamDriverSelect from '../components/TeamDriverSelect.svelte';
  import GameScreen from '../components/GameScreen.svelte';

  let currentStep = 'title'; // 'title' | 'select' | 'game'
  let selectedTeam = null;
  let selectedDriver = null;

  function handleStart() {
    currentStep = 'select';
  }

  function handleSelectionConfirmed(event) {
    selectedTeam = event.detail.team;
    selectedDriver = event.detail.driver;
    currentStep = 'game';
  }
</script>

{#if currentStep === 'title'}
  <TitleScreen on:start={handleStart} />
{:else if currentStep === 'select'}
  <TeamDriverSelect on:selectionConfirmed={handleSelectionConfirmed} />
{:else if currentStep === 'game'}
  <GameScreen team={selectedTeam} driver={selectedDriver} />
{/if}
