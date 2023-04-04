<script>
  import {stratify, hierarchy, tree} from "d3-hierarchy"
  import {curveBumpX as curve, link} from "d3-shape"
  import Root from "./Root.svelte"
  export let data 

function aggregateData(data) {
  // parser for flat array to tree
  const stratifier = stratify(data)
    .id(d => d.id)
    .parentId((d) => d.parentId);

  // parse hierarchical tree
  const rootNode = stratifier(data);
  return rootNode;
}

$: treeData = aggregateData(data);
$: console.log({treeData});
$: hierarchyData = hierarchy(treeData)

/* graph */
$: treeLayout = tree().size([(height / 2) - 10, width - 10]);
$: calc = treeLayout(hierarchyData)
$: drawCurve = link(curve)
            .x(d => d.y)
            .y(d => d.x)

let width, height
</script>
<svelte:window bind:innerHeight={height} bind:innerWidth={width} />
<code>{width} x {height}</code>
<main>
  {#if treeData}
    <Root root={treeData} />
    <hr>
    <svg width={width} height={height / 2}>
      <g transform="translate(5, 5)">
        {#each calc.descendants() as node}
          <circle cx={node.y} cy={node.x} r=5 fill="yellowgreen"><title>{node?.data?.data?.label}</title></circle>
        {/each}
        {#each calc.links() as edge}
          <path d={drawCurve(edge)} stroke="white" stroke-width="1" fill="none" />
        {/each}
      </g>
    </svg>
  {/if}
</main>

<style>
  svg {
    overflow: hidden;
  }
</style>