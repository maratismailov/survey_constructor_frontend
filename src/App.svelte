<script>
  import { Route, active } from "tinro";
  import Constructor from "./pages/Constructor.svelte";
  import Survey from "./pages/Survey.svelte"
</script>

<main>
  <div class="scrollmenu">
    <a href="/" use:active exact>constructor</a>
    <a href="/survey" use:active exact>survey</a>
  </div>
</main>

<Route path="/">
  <Constructor />
</Route>

<Route path="/survey">
  <Survey />
</Route>