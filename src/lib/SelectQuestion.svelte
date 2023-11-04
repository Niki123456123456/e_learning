<script lang="ts">
    import Katex from "svelte-katex";
    let all_answers_correct = undefined;
    let qa = {
        question:
            "\\text{Was ist} \\space \\textcolor{red}{a+a} \\space \\text{?}",
        answers: [
            {
                text: "a",
                is_correct: true,
                is_selected: false,
            },
            {
                text: "0",
                is_correct: false,
                is_selected: false,
            },
            {
                text: "1",
                is_correct: false,
                is_selected: false,
            },
        ],
    };

    function check_answers() {
        for (let i = 0; i < qa.answers.length; i++) {
            if (qa.answers[i].is_correct != qa.answers[i].is_selected) {
                all_answers_correct = false;
                return;
            }
        }
        all_answers_correct = true;
        console.log("test");
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
<button on:click={check_answers}>Prüfen</button>

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
