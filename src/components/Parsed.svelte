<script>
  export let survey_body;
</script>

{#each survey_body as element}
  <div>{element.id}</div>
  <div>{element.name_ru}</div>
  {#if element.type === 'text'}
    <input bind:value={element.value} type="text" />
    <br />
  {:else if element.type === 'number'}
    <input bind:value={element.value} type="number" />
    <br />
  {:else if element.type === 'select'}
    <!-- <input bind:value={element.value} type="number" /> -->
    <select>
      <option>option1</option>
      <option>option2</option>
    </select>
    <br />
  {:else if element.type === 'table'}
    {#each element.fields as field} 
        
    {/each}
    <br />
  {/if}
{/each}
