<script>
  import Parsed from '../components/Parsed.svelte'

  let survey = [];
  let surveys = []
  if (localStorage.getItem("survey") !== "") {
    survey = JSON.parse(localStorage.getItem("survey"));
  }
  if (localStorage.getItem("surveys") === null) {
    localStorage.setItem('surveys', '[]')
    surveys = JSON.parse(localStorage.getItem("surveys"));
  }
  let pretty_survey;
  let parsed_body = [];

  const parse_body = () => {
    parsed_body = new Parse(survey.survey_body);
  };

  const show_survey = () => {
    pretty_survey = JSON.stringify(survey, undefined, 2);
  };

  const save_survey = () => {
    surveys.push(survey)
    localStorage.setItem('surveys', JSON.stringify(surveys))
  }
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

<!-- {#each parsed_body as survey_element}
  <div>
    {@html survey_element}
  </div>
{/each} -->
<Parsed survey={survey.survey_body}/>


<hr />

<button on:click={show_survey}>show survey</button>
<button on:click={parse_body}>parse survey</button>
<button on:click={save_survey}>save survey</button>

<hr />
{#if pretty_survey}
  <pre>{pretty_survey}</pre>
{/if}
