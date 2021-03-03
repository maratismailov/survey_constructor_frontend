<script>
  let element = {
    id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    type: "",
    fields: []
  };
  let column = {
    id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    type: ""
  };
  let column_types = ["choose type", "text", "number", "select"];
  let table_form = false;
  let type_is_text = false;
  let type_is_number = false;
  let type_is_select = false;
  let type_is_table = false;
  let element_type = "";

  let parsed_body = [];
  let parsed_table = [];
  let pretty_element = "";
  let pretty_survey = "";
  let form_is_active = false;
  let table_fields = [];
  let types = ["choose type", "text", "number", "select", "table"];
  let survey_body = [];
  let add_element;
  const survey_name = "nana";

  const parse_body = body => {
    const parsed = body.map(elem => {
      if (elem.type == "text") {
        return (
          "<div>" +
          elem.id +
          "</div><div>" +
          elem.name_ru +
          "</div><div>" +
          elem.name_en +
          "</div><div>" +
          elem.name_kg +
          "</div><br>" +
          "<input type='text'/>"
        );
      } else if (elem.type == "select") {
        return (
          "<div>" +
          elem.name +
          "</div><br>" +
          "<select> <option> option1 </option> <option> option2 </option> </select>"
        );
      } else if (elem.type == "number") {
        return "<div>" + elem.name + "</div><br>" + "<input type='number'/>";
      } else if (elem.type == "table") {
        const regexp_table = /","|\["|"]/g;
        const cleaned_table = JSON.stringify(parse_body(elem.fields)).replace(
          regexp_table,
          ""
        );
        console.log(parse_body(elem.fields));
        return (
          '<div style="margin-left:35px">' +
          elem.name +
          cleaned_table +
          "</div>"
        );
      }
    });
    return parsed;
  };
  const submit_element = () => {
    console.log(element);
    survey_body.push(element);
    parsed_table = [];
    survey_body = survey_body;
    parsed_body = parse_body(survey_body);
    // parsed_body = parsed_body;
    element = {
      id: "",
      name_ru: "",
      name_en: "",
      name_kg: "",
      type: "",
      fields: []
    };
    column = {
      id: "",
      name_ru: "",
      name_en: "",
      name_kg: "",
      type: ""
    };
  };
  const show_element = () => {
    pretty_element = JSON.stringify(element, undefined, 2);
  };
  const show_survey = () => {
    pretty_survey = JSON.stringify(survey_body, undefined, 2);
  };
  const add_column = () => {
    element.fields.push(column);
    parsed_table = parse_body(element.fields);
    column = {
      name: "",
      type: ""
    };
    document.getElementById("select_type").selectedIndex = 0;
  };
</script>

<style>
  .form_element {
    display: flex;
  }
  .form_left {
    width: 170px;
  }
  .form_right {
    flex-grow: 1;
  }
  .table {
    margin: 5px;
    margin-left: 25px;
  }
  .select_type {
    display: flex;
  }
  .type_radio {
    margin: 5px;
  }
</style>

<h1>Survey name is {survey_name}</h1>

{#each parsed_body as survey_element}
  <div>
    {@html survey_element}
  </div>
{/each}
<hr />

<!-- <button on:click={add_survey_element}>add survey element</button> -->

<!-- {#if form_is_active} -->
<form on:submit|preventDefault={submit_element}>
  <div class="form_element">
    <div class="form_left">element id</div>
    <div class="form_right">
      <input bind:value={element.id} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">element name ru</div>
    <div class="form_right">
      <input bind:value={element.name_ru} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">element name en</div>
    <div class="form_right">
      <input bind:value={element.name_en} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">element name kg</div>
    <div class="form_right">
      <input bind:value={element.name_kg} type="text" />
    </div>
  </div>
  <!-- <div class="form_element">
    <div class="form_left">element type</div>
    <div class="form_right">
      <select bind:value={element.type}>
        {#each types as type}
          <option value={type}>{type}</option>
        {/each}
      </select>
    </div>
  </div> -->

  <div>
    {#if element.type == 'table'}
      <!-- <div class="form_element">
          <div class="form_left">rows</div>
          <div class="form_right">
            <input bind:value={table_rows} type="number" />
          </div>
        </div> -->

      <div>fields:</div>
      <div>
        {#each parsed_table as table_element}
          <div class="table">
            {@html table_element}
          </div>
        {/each}
        <input bind:value={column.name} placeholder="name" type="text" />
        <!-- <select id="select_type" bind:value={column.type}>
          {#each column_types as type}
            <option value={type}>{type}</option>
          {/each}
        </select> -->
        <div class="select_type">
          <label class="type_radio">
            <input type="radio" bind:group={column.type} value={'text'} />
            text
          </label>
          <label class="type_radio">
            <input type="radio" bind:group={column.type} value={'number'} />
            number
          </label>
          <label class="type_radio">
            <input type="radio" bind:group={column.type} value={'select'} />
            select
          </label>
          <button type="button" on:click={add_column}>add column</button>
          <button type="submit" style="margin-left: 5px">Submit</button>
        </div>
      </div>
    {/if}
  </div>
  {#if element.type != 'table'}
    <div class="select_type">
      <label class="type_radio">
        <input type="radio" bind:group={element.type} value={'text'} />
        text
      </label>
      <label class="type_radio">
        <input type="radio" bind:group={element.type} value={'number'} />
        number
      </label>
      <label class="type_radio">
        <input type="radio" bind:group={element.type} value={'select'} />
        select
      </label>
      <label class="type_radio">
        <input type="radio" bind:group={element.type} value={'table'} />
        table
      </label>
      <button type="submit">Submit</button>
    </div>
  {/if}
</form>
<!-- {/if} -->
<button on:click={show_element}>show element</button>
<button on:click={show_survey}>show survey</button>

{#if pretty_element}
  <pre>{pretty_element}</pre>
{/if}

{#if pretty_survey}
  <pre>{pretty_survey}</pre>
{/if}
