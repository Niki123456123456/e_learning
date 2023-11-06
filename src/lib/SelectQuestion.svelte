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
        <div style="display: inline-block;"><Katex displayMode>{answer.text}</Katex></div>
        
    </div>
{/each}

<style>
    .answer_selected {
        background-color: #1a1a1a;
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

    .answer_not_selected_wrong,
    .answer_selected_wrong,
    .answer_selected_correct,
    .answer_not_selected_wrong,
    .answer,
    .answer_selected {
        border-radius: 8px;
    }

    .answer:hover {
        background-color: #1a1a1a;
    }

    .answer_selected::before, .answer:hover::before {
        content: url("/cross.svg");
    }
</style>
