<script lang="ts">
  import CodeMirror from "svelte-codemirror-editor";
  import { oneDark } from "@codemirror/theme-one-dark";
  import { basicSetup } from "codemirror";
  import { autocompletion, completeFromList, type Completion } from "@codemirror/autocomplete";
  import Katex from "svelte-katex";

  function completion(insert : string, label :string) : Completion{
    return {
      label: label,
          apply(view, completion, from, to) {
            let index = insert.indexOf('?');
            let ins = insert.replace("?", "");
            if (index < 0 ){
              index = ins.length
            }
            view.dispatch({
              changes: {
                from: from,
                to: to,
                insert: ins,
              },
              selection: {
                anchor: from + index,
                head: from + index,
              }
            });
          },
    };
  }

  let test2 = autocompletion({
    override: [
      completeFromList([
        completion("\\in", "in"),
        completion("\\approx", "approx"),
        completion("\\times", "times"),
        completion("\\vec{?}", "vec"),
        completion("\\sum_{i=1}^n", "sum"),
        completion("\\prod_{i=1}^n", "prod"),
        completion("\\sqrt[]{?}", "sqrt"),
        completion("\\frac{?}{}","frac"),
        completion("\\lor", "or"),
        completion("\\land", "and"),
        completion("\\forall", "forall"),
        completion("\\lnot", "not"),
        completion("\\tilde{?}", "tilde"),
        completion("\\bar{?}", "bar"),
      ]),
    ],
  });

  let value = "";
</script>

<main>
  <CodeMirror
    bind:value
    extensions={[basicSetup, test2]}
    theme={oneDark}
    styles={{
      "&": {
        width: "500px",
        maxWidth: "100%",
        height: "100px",
        textAlign: "left",
      },
    }}
  />
  <Katex displayMode>{value}</Katex>
</main>
