<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from "gsap";
  import { PixiPlugin } from "gsap/PixiPlugin";
  gsap.registerPlugin(PixiPlugin);

  export let title: string;
  export let description: string;
  export let imageUrl: string;
  export let onClick: () => void;

  let cardEl: HTMLDivElement;

  function handleClick() {
    if (title.includes('Attack')) {
      animateAttack();
    } else if (title.includes('Defend')) {
      animateDefend();
    } else {
      animatePush();
    }

    onClick(); // trigger passed in logic
  }

  function animateAttack() {
    gsap.fromTo(cardEl, 
      { scale: 1, backgroundColor: "#222", boxShadow: "0 0 0px red" }, 
      {
        scale: 1.1,
        backgroundColor: "#440000",
        boxShadow: "0 0 20px red",
        duration: 0.3,
        yoyo: true,
        repeat: 1,
        ease: "power2.out"
      }
    );
  }

  function animateDefend() {
    gsap.fromTo(cardEl, 
      { scale: 1, backgroundColor: "#222", boxShadow: "0 0 0px blue" }, 
      {
        scale: 0.95,
        backgroundColor: "#001133",
        boxShadow: "0 0 16px cyan",
        duration: 0.3,
        yoyo: true,
        repeat: 1,
        ease: "power1.inOut"
      }
    );
  }

  function animatePush() {
    gsap.fromTo(cardEl,
      { scale: 1, backgroundColor: "#222", boxShadow: "0 0 0px orange" },
      {
        scale: 1.15,
        rotate: 2,
        backgroundColor: "#332100",
        boxShadow: "0 0 18px orange",
        duration: 0.3,
        yoyo: true,
        repeat: 1,
        ease: "back.out(1.7)"
      }
    );
  }
</script>

<style>
  .card {
    border: 1px solid white;
    border-radius: 8px;
    padding: 1rem;
    cursor: pointer;
    width: 180px;
    min-height: 260px;
    background: #222;
    color: white;
    user-select: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: box-shadow 0.2s ease;
  }

  .title {
    font-weight: bold;
    font-size: 1.2rem;
    margin-bottom: 0.75rem;
    text-align: center;
    width: 100%;
  }

  img {
    width: 120px;
    height: 130px;
    object-fit: contain;
    margin-bottom: 0.75rem;
    border-radius: 6px;
  }

  .desc {
    font-size: 0.85rem;
    color: #ccc;
    text-align: center;
    width: 100%;
    flex-grow: 1;
  }
</style>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<div bind:this={cardEl} class="card" on:click={handleClick}>
  <div class="title">{title}</div>
  <img src={imageUrl} alt={title} />
  <div class="desc">{description}</div>
</div>
