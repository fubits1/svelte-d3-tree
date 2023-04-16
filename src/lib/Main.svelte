<script>
  import {stratify, hierarchy, tree} from "d3-hierarchy"
  import {curveBumpX as curve, link} from "d3-shape"
  import Root from "./Node.svelte"
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
// $: console.log({treeData});
$: hierarchyData = hierarchy(treeData)

/* graph */
const padding = 20
$: treeLayout = tree().size([(height / 2) - padding, width - padding]);
$: rootNode = treeLayout(hierarchyData)
$: drawCurve = link(curve)
            .x(d => d.y)
            .y(d => d.x)

let width, height
</script>
<svelte:window bind:innerHeight={height} bind:innerWidth={width} />
<code>{width} x {height}</code>
<main>
  {#if treeData}
    <Root node={treeData} />
    <hr>
    <svg width={width} height={height / 2}>
      <g transform="translate({padding / 2}, {padding / 2})">
        {#each rootNode.descendants() as node}
          {@const label = node?.data?.data?.label}
          {@const x = node.x}
          {@const y = node.y}
          <g class="entity">
            <circle cx={y} cy={x} r=5 fill="yellowgreen"><title>{label}</title></circle>
            <!-- <text x={y} y={x} text-anchor="middle" dy="-10" stroke="#242424" paint-order="stroke" fill="yellowgreen" font-size="10">{label}</text> -->
            <foreignObject x={y - 50} y={x - 20} height="20" width="100"><div class="label">{label}</div></foreignObject>
          </g>
        {/each}
        {#each rootNode.links() as edge}
          <path d={drawCurve(edge)} stroke="slategrey" stroke-width="1" fill="none" />
        {/each}
      </g>
    </svg>
  {/if}
</main>

<style>
  svg {
    overflow: hidden;
  }

  .label {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
</style>