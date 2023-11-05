<script lang="ts">
    import Katex from "svelte-katex";
    export let all_answers_correct;
    export let qa;

    export function check_answers() {
        for (let i = 0; i < qa.answers.length; i++) {
            if (qa.answers[i].is_correct != qa.answers[i].is_selected) {
                return false;
            }
        }
        return true;
    }
</script>

<Katex displayMode>{qa.question}</Katex>
{#each qa.answers as answer}
    <div
        on:click={() => {
            if (all_answers_correct === undefined) {
                answer.is_selected = answer.is_selected === false;
            }
        }}
        class={all_answers_correct !== undefined
            ? answer.is_selected
                ? answer.is_selected == answer.is_correct
                    ? "answer_selected_correct"
                    : "answer_selected_wrong"
                : answer.is_selected == answer.is_correct
                    ? "answer"
                    : "answer_not_selected_wrong"
            : answer.is_selected
            ? "answer_selected"
            : "answer"}
    >
        <Katex displayMode>{answer.text}</Katex>
    </div>
{/each}

<style>
    .container {
        display: flex;
    }

    .answer_selected {
        background-color: black;
    }
    .answer_selected_correct {
        background-color: darkgreen;
    }
    .answer_selected_wrong {
        background-color: darkred;
    }
    .answer_not_selected_wrong {
        background-color: lightgreen;
    }
</style>
