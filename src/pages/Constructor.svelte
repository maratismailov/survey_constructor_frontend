<script>
  import Parsed from "../components/Parsed.svelte";

  let surveys = [];
  let survey = {
    survey_id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    survey_body: []
  };
  const blank_element = {
    id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    type: "",
    value: "",
    fields: []
  };
  let element = { ...blank_element };

  const blank_column = {
    id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    type: "",
    value: ""
  };
  let column = { ...blank_column };

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
  if (
    localStorage.getItem("survey") !== "" &&
    localStorage.getItem("survey") !== null
  ) {
    survey = JSON.parse(localStorage.getItem("survey"));
  } else {
    survey = survey;
  }
  if (localStorage.getItem("surveys") === null) {
    localStorage.setItem("surveys", "[]");
    surveys = JSON.parse(localStorage.getItem("surveys"));
  }
  let add_element;
  const survey_name = "nana";

  const submit_element = () => {
    console.log("blank", blank_element);
    console.log(element);
    survey.survey_body.push(element);
    parsed_table = [];
    survey = survey;
    // parsed_body = new Parse(survey.survey_body);
    // parsed_body = parsed_body;
    element = { ...blank_element };
    element.fields = [];
    column = { ...blank_column };
  };
  const show_element = () => {
    pretty_element = JSON.stringify(element, undefined, 2);
  };
  const show_survey = () => {
    pretty_survey = JSON.stringify(survey, undefined, 2);
  };
  const save_survey_template = () => {
    localStorage.setItem("survey", JSON.stringify(survey));
  };
  const save_survey = () => {
    localStorage.setItem("survey", JSON.stringify(survey));
  };
  const delete_survey = () => {
    localStorage.setItem("survey", "");
  };
  const add_column = () => {
    element.fields.push(column);
    column = { ...blank_column };
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

<!-- <h1>Survey id is {survey_root.survey_id}</h1>
<h1>Survey names are {survey_root.name_ru} {survey_root.name_en} {survey_root.name_kg}</h1> -->

<Parsed survey={survey.survey_body} />
<hr />

<!-- <button on:click={add_survey_element}>add survey element</button> -->

<!-- {#if form_is_active} -->

<form on:submit|preventDefault={submit_element}>
  <div class="form_element">
    <div class="form_left">survey id</div>
    <div class="form_right">
      <input bind:value={survey.survey_id} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">survey name ru</div>
    <div class="form_right">
      <input bind:value={survey.name_ru} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">survey name en</div>
    <div class="form_right">
      <input bind:value={survey.name_en} type="text" />
    </div>
  </div>

  <div class="form_element">
    <div class="form_left">survey name kg</div>
    <div class="form_right">
      <input bind:value={survey.name_kg} type="text" />
    </div>
  </div>
</form>
<hr />
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
      <div>fields:</div>
      <div>
        {#each parsed_table as table_element}
          <div class="table">
            {@html table_element}
          </div>
        {/each}
        <input bind:value={column.id} placeholder="name" type="text" />
        <div id="select_type" class="select_type">
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
<button on:click={save_survey}>save survey</button>
<button on:click={save_survey_template}>save survey template</button>
<button on:click={delete_survey}>delete survey</button>

{#if pretty_element}
  <pre>{pretty_element}</pre>
{/if}

{#if pretty_survey}
  <pre>{pretty_survey}</pre>
{/if}
