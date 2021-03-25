<script>
  export let element;
  let selected

  const options = ['option1', 'option2']
</script>

<select bind:value={element.value}>
  <option>option1</option>
  <option>option2</option>
</select>
<br />
