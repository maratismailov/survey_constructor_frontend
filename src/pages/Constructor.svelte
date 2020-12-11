<script>
  let element = {
    name: "",
    type: "",
    values: []
  };
  let parsed_body = [];
  let form_is_active = false;
  let types = ["choose type", "text", "number", "select"];
  let survey_body = [];
  let add_element;
  const survey_name = "nana";
  const add_survey_element = () => {
    form_is_active = true;
  };
  const parse_body = () => {
    parsed_body = survey_body.map(elem => {
      console.log(elem);
      if (elem.type == "text") {
        return "<div>" + elem.name + "</div><br>" + "<input type='text'/>";
      } else if (elem.type == "select") {
        return (
          "<div>" +
          elem.name +
          "</div><br>" +
          "<select /> <option> option1 </option> <option> option2 </option> </select>"
        );
      } else if (elem.type == "number") {
        return "<div>" + elem.name + "</div><br>" + "<input type='number'/>";
      }
    });
  };
  const submit_element = () => {
    form_is_active = false;
    survey_body.push(element);
    survey_body = survey_body;
    parse_body();
    parsed_body = parsed_body;
    element = {
      name: "",
      type: "",
      values: []
    };
  };
</script>

<h1>Survey name is {survey_name}</h1>

{#each parsed_body as survey_element}
  <div>
    {@html survey_element}
  </div>
{/each}
<hr />

<button on:click={add_survey_element}>add survey element</button>
<hr />

{#if form_is_active}
  <form on:submit|preventDefault={submit_element}>
    <div>
      element name
      <input bind:value={element.name} type="text" />
    </div>
    <div>
      element type
      <select bind:value={element.type}>
        {#each types as type}
          <option value={type}>{type}</option>
        {/each}
      </select>
    </div>
    <button type="submit">Submit</button>
  </form>
{/if}
