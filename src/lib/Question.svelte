<script lang="ts">
    import { onMount } from "svelte";
    import TextInput from "./TextInput.svelte";
    import SelectQuestion from "./SelectQuestion.svelte";

    let clear_func;
    let check_answers_func;
    let all_answers_correct = undefined;
    let qa;

    onMount(async () => {
        await get_next_question();
    });

    async function get_next_question() {

        // http://167.71.46.2:3000/ http://localhost:8080/
        const response = await fetch("http://167.71.46.2:3000/");
        qa = await response.json();
        all_answers_correct = undefined;
        if (clear_func) {
            clear_func();
        }
    }

    function check_answers() {
        all_answers_correct = check_answers_func();
    }
</script>

{#if qa}
    <div>
        {#if qa.type === "textinput"}
            <TextInput
                bind:check_answers={check_answers_func}
                bind:clear={clear_func}
                {qa}
                {all_answers_correct}
            />
        {:else}
            <SelectQuestion
                bind:check_answers={check_answers_func}
                {qa}
                {all_answers_correct}
            />
        {/if}
    </div>
    {#if all_answers_correct === undefined}
        <button on:click={check_answers}>Prüfen</button>
    {:else}
        <button on:click={get_next_question}>Nächste Frage</button>
    {/if}
{:else}
    <p>Loading ...</p>
{/if}
