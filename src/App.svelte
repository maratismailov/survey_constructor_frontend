<script>
  import { Route, active } from "tinro";
  import Constructor from "./pages/Constructor.svelte";
  import Survey from "./pages/Survey.svelte"
</script>

<main>
  <div class="scrollmenu">
    <a href="/rent/surveyeditor" use:active exact>constructor</a>
    <a href="/rent/surveyeditor/survey" use:active exact>survey</a>
  </div>
</main>

<Route path="/rent/surveyeditor">
  <Constructor />
</Route>

<Route path="/rent/surveyeditor/survey">
  <Survey />
</Route>