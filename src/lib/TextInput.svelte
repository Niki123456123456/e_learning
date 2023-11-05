<script lang="ts">
    import Katex from "svelte-katex";

    export let all_answers_correct;
    export let qa;
    let answer = "";

    export function check_answers() {
        return answer === qa.answer;
    }

    export function clear() {
        answer = "";
    }
</script>

<Katex displayMode>{qa.question}</Katex>
<input
    bind:value={answer}
    placeholder="gebe deine Anwort ein"
    readonly={all_answers_correct !== undefined}
/>
{#if all_answers_correct !== undefined}
    {#if all_answers_correct === true}
        <p>richtig!</p>
    {:else}
        <p>leider falsch.</p>
    {/if}
    <Katex displayMode>{qa.explanation}</Katex>
{/if}
