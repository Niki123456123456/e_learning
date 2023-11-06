<script lang="ts">
    import KatexInput from "./KatexInput.svelte";
    let question = "";
    let answers = [
        {
            text: "",
            is_correct: false,
        },
    ];

    function add_option() {
        answers.push({
            text: "",
            is_correct: false,
        });
        answers = answers;
    }

    function remove_option(i : number){
        answers.splice(i, 1);
        answers = answers;
    }
</script>

<KatexInput value={question} />
{#each answers as answer, i (i)}
    <input type="checkbox" bind:checked={answer.is_correct} />
    <button on:click={() => {remove_option(i);}}>x</button>
    <KatexInput value={answer.text} />
{/each}
<button on:click={add_option}>add option</button>
