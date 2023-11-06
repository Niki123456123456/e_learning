<script lang="ts">
    import Question from "./lib/Question.svelte";
    import AddQuest from "./lib/AddQuest.svelte";
    import Router from 'svelte-spa-router'

    const routes = {
    '/addquest': AddQuest,
    '*': Question,
}
</script>

<main>
  <Router {routes}/>
</main>