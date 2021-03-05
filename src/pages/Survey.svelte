<script>
  let survey_body = [];
  if (localStorage.getItem("survey_body") !== "") {
    survey_body = JSON.parse(localStorage.getItem("survey_body"));
  }
  let pretty_survey;
  let parsed_body = [];

  const parse_body = () => {
    parsed_body = parse(survey_body);
  };
  const parse = () => {
    const parsed = survey_body.map(elem => {
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
        return (
          '<div style="margin-left:35px">' +
          elem.name +
          cleaned_table +
          "</div>"
        );
      } else if (elem.type == "root") {
        return (
          "<h1>Survey id is" +
          elem.survey_id +
          "</h1><h1>Survey names are " +
          elem.name_ru +
          elem.name_en +
          elem.name_kg +
          "</h1>"
        );
      }
    });
    return parsed;
  };

  const show_survey = () => {
    pretty_survey = JSON.stringify(survey_body, undefined, 2);
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

{#each parsed_body as survey_element}
  <div>
    {@html survey_element}
  </div>
{/each}
<hr />

<button on:click={show_survey}>show survey</button>
<button on:click={parse_body}>parse survey</button>

<hr />
{#if pretty_survey}
  <pre>{pretty_survey}</pre>
{/if}
