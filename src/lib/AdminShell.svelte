<script>
  import Icon from '@iconify/svelte'

  export let data = undefined
  /**@type {object[]}*/
  export let constructors = []
  export let activeConstructorIndex = 0
  
  let editing = false
</script>

<div class="admin-shell">
  <div class="edit-icons">
    <Icon icon="mdi:edit-circle-outline" class="edit-icon" />
  </div>
  
  {#if editing}
    <div class="icon-rail">
      <slot name="iconRail" />
    </div>
  {/if}

  <div class="inner">
    {#if constructors.length === 1}
      <svelte:component this={constructors[0].constructor}></svelte:component>
    {:else}
      <slot />
    {/if}
  </div>
</div>

<style lang="scss">
  .admin-shell {
    position: relative;
    width: 100%;

    .edit-icons {
      font-size: 2rem;
      position: absolute;
      top: 0;
      right: 0;
    }
  }
</style>