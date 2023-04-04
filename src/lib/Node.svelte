<script>
export let node
const isParent = node?.children?.length
</script>

{#if isParent}
  <details open>
    <summary>{node?.data?.label} {#if isParent}[{node?.children?.length}]{/if}</summary>
    {#if isParent}
      {#each node.children as child}
        <svelte:self node={child}/>
      {/each}
    {/if}
  </details>
{:else}
  <div class="leaf">{node?.data?.label}</div>
{/if}

<style>
  details {
    padding-left: 1rem;
  }

  summary {
    font-weight: 700;
    font-size: 1.1rem;
  }

  .leaf {
    padding-left: 2rem;
  }
</style>
