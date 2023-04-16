<script>
import Node from "./Node.svelte"
export let root
</script>

<details open>
  <summary>Root [{root?.children?.length}]</summary>
  {#each root?.children as node}
    {#if !node?.parentId}
      <Node {node} />
    {/if}
  {/each}
</details>

<style>
  details {
    padding-left: 1rem;
  }

  summary {
    font-weight: 700;
    font-size: 1.4rem;
  }
</style>
