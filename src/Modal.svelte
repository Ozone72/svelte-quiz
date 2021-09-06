<script>
  import { fly, fade } from "svelte/transition";
  // The event dispatcher allows you to trigger events on components
  import { createEventDispatcher } from "svelte";
  let w;
  const dispatch = createEventDispatcher();
</script>

<!-- bind: lets you bind different aspects of measurement to a variable and output -->
<div class="modal-bg" transition:fade bind:clientWidth={w}>
  <div class="modal" transition:fly={{ y: -100 }}>
    <!-- This button will dispatch or emit an event called close -->
    <button
      on:click={() => {
        dispatch("close");
      }}
    >
      Close
    </button>
    <!-- <slot>optional fallback</slot> -->
    {w + "px"}
    <slot />
    <!-- <slot name="modal" /> -->
  </div>
</div>

<style lang="scss">
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.8);
    .modal {
      background: white;
      padding: 20px;
      border-radius: 15px;
    }
  }
</style>
