<script>
  let parsed_body = ["<div>empty survey element</div>", "<div>empty survey element 2</div>"];
  let survey_body = [
    { name: "first row", type: "text" },
    {
      name: "second row",
      type: "select",
      values: ["first option", "second option", "third option"]
    },
    {
      name: "third row",
      type: "integer"
    }
  ];
  const survey_name = "nana";
  const parse_body = () => {
    parsed_body = survey_body.map(elem => {
      if(elem.type == 'text') {
        return('<input type=\'text\'/>')
      }
      else if(elem.type == 'select') {
        return('<select /> <option> option1 </option> <option> option2 </option> </select>')
      }
      else if(elem.type == 'number') {
        return('<input type=\'number\'/>')
      }
    })
  };
</script>

<h1>Survey name is {survey_name}</h1>

<button on:click={parse_body}>parse body</button>

{#each parsed_body as survey_element}
  <div>
    {@html survey_element}
  </div>
{/each}
