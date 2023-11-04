<script lang="ts">
  import CodeMirror from "svelte-codemirror-editor";
  import { color, oneDark } from "@codemirror/theme-one-dark";
  import { basicSetup } from "codemirror";
  import { autocompletion, completeFromList, type Completion } from "@codemirror/autocomplete";
  import Katex from "svelte-katex";
  import {gutter} from "@codemirror/view"

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
        completion("\\space", "space"),
        completion("\\cdot", "dot"),
        completion("\\binom{?}{}", "binom"),
        completion("\\set{?}","set"),
        completion("\\textcolor{?}{}", "color"),
        completion("\\infty","infinity"),
        completion("\\N","N"), // natürliche Zahlen 0, 1, 8
        completion("\\Z","Z"), // ganze Zahlen -1
        completion("\\mathbb{Q}","Q"), // rationale Zahlen 0.6
        completion("\\mathbb{I}","I"), // irrationale Zahlen \sqrt{2}
        completion("\\R","R"), // reelle Zahlen
        completion("\\mathbb{C}","C"), // komplexe Zahlen


      ]),
    ],
  });

  let value = "";
</script>

<main>
  <CodeMirror
    bind:value
    extensions={[basicSetup, test2, gutter({
      lineMarker: null,
    })]}
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
