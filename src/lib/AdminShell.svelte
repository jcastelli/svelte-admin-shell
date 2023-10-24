<script>
  import {fade} from 'svelte/transition'

  export let data = undefined
  /**@type {object[]}*/
  export let componentGroupTitle = undefined
  export let constructors = undefined
  export let activeConstructorIndex = 0
  export let editable
  let editing = false
  let previousComponent = undefined

  const navigateToComponentIndex = (fromIndex, toIndex)=> {
    activeConstructorIndex = toIndex
  }
  
  const chooseNextComponent = ()=> {
    const outgoingComponentIndex = activeConstructorIndex
    if(constructors.length && constructors.length > 1) {
      if(outgoingComponentIndex === constructors.length - 1) {
        navigateToComponentIndex(outgoingComponentIndex, 0)
      } else {
        navigateToComponentIndex(outgoingComponentIndex, outgoingComponentIndex + 1)
      }
    } else {
      console.error(`Something is wrong with the component choices, and we cannot navigate to next${componentGroupTitle ? ':'+componentGroupTitle : '' } `)
    }
  }
  
  const choosePreviousComponent = ()=> {
    const outgoingComponentIndex = activeConstructorIndex
    if(constructors.length && constructors.length > 1) {
      if(outgoingComponentIndex === 0) {
        navigateToComponentIndex(outgoingComponentIndex, constructors.length - 1)
      } else {
        navigateToComponentIndex(outgoingComponentIndex, outgoingComponentIndex - 1)
      }
    } else {
      console.error(`Something is wrong with the component choices, and we cannot navigate to previous${componentGroupTitle ? ':'+componentGroupTitle : '' } `)
    }
  }

  $: isEditing = editing && editable ? true : false
</script>

<div class="shell" class:actively-editing={isEditing} class:editable={editable}>
  {#if editable}
    <div class="edit-icons">
      <button on:click={()=> editing = !editing}>
        {#if !isEditing}
          <span class="edit-ui" in:fade>
            Edit >
          </span>
        {:else}
          <span class="close-ui" in:fade>
            Close X
          </span>
        {/if}
      </button>
    </div>
  {/if}
  
  {#if isEditing}
    <div class="action-rail" transition:fade>
      {#if constructors && constructors.length > 1}
        <div class="component-selector">
          <span>Choose a {componentGroupTitle ? componentGroupTitle : 'component'}:</span>
          <select bind:value={activeConstructorIndex} on:change={(thing)=> console.log(thing)}>
            {#each constructors as _, i}
              <option 
                name="{_.hasOwnProperty("name") ? _.name : _.constructor?.name}"
                value="{i}"
              >
                {_.title}
              </option>
            {/each}
          </select>

          <button class="next" on:click={chooseNextComponent}>
            Next
          </button>
          <button class="previous" on:click={choosePreviousComponent}>
            Previous
          </button>
        </div>
      {/if}

      <slot name="iconRail" />
    </div>
  {/if}

  <div class="inner">
    {#if constructors && constructors.length === 1}
      <svelte:component this={constructors[0].constructor} {data}></svelte:component>
    {:else if constructors}
      <svelte:component this={constructors[activeConstructorIndex].constructor} {data}></svelte:component>
    {/if}
    <slot />
  </div>
</div>

<style lang="scss"> 
  .shell {
    position: relative;
    width: 100%;
    overflow: hidden;
    background-color: #333;
    font-size: 1rem;
    line-height: 1;
    box-sizing: border-box;
    
    &.editable:hover .edit-icons {
      opacity: 1;
    }

    .edit-icons {
      padding: 0.2rem;
      position: absolute;
      top: 0;
      right: 0;
      opacity: 0;
      transition: opacity 0.5s;

      button {
        cursor: pointer;
        padding: 0.5rem 1rem;
        margin: 0;
        background-color: #333;
        opacity: 90%;
        color: white;
        z-index: 20;
        border-radius: 0.5rem;
        border: 1px solid #333;
        font-size: 1rem;
        transition: 0.2s ease-in-out;
      }
    }

    .inner {
      transition: 0.2s ease-in-out;
    }

    &.actively-editing {
      
      .inner {
        transform: translate(0, 2.5rem);
        scale: 90%;
        opacity: 20%;
        mix-blend-mode: luminosity;
      }
    }

    .action-rail {
      position: absolute;
      top: 0;
      left:0;
    }
  }
</style>