<script>
  import ArrowDropDown from "../../../img/ArrowDropDown.svelte";
  import FlagFurtherRead from "../../../img/FlagFurtherRead.svelte";
  import FlagIdea from "../../../img/FlagIdea.svelte";
  import FlagImportant from "../../../img/FlagImportant.svelte";
  import FlagImpTerm from "../../../img/FlagImpTerm.svelte";
  import FlagQuestion from "../../../img/FlagQuestion.svelte";
  import FlagReference from "../../../img/FlagReference.svelte";
  import FlagRevisit from "../../../img/FlagRevisit.svelte";
  import FlagToDo from "../../../img/FlagToDo.svelte";
  import FlagUrgent from "../../../img/FlagUrgent.svelte";
  import IcoFlag from "../../../img/IcoFlag.svelte";
  import IcoChecked from "../../../img/OutlineIcoChecked.svelte";
  import { page } from "../../../../stores";

  export let addArrParams;

  $: params = $page.query;

  let flags = [
    "furtherreading",
    "idea",
    "important",
    "importantterm",
    "question",
    "reference",
    "revisit",
    "todo",
    "urgent",
  ];

  function assignIco(icon) {
    switch (icon) {
      case "furtherreading":
        return FlagFurtherRead;
      case "idea":
        return FlagIdea;
      case "important":
        return FlagImportant;
      case "importantterm":
        return FlagImpTerm;
      case "question":
        return FlagQuestion;
      case "reference":
        return FlagReference;
      case "revisit":
        return FlagRevisit;
      case "todo":
        return FlagToDo;
      case "urgent":
        return FlagUrgent;
    }
  }
</script>

<style>
  .FilterFlags li {
    grid-template-columns: max-content 1fr 13px;
  }
  .FilterFlags li :global(svg) {
    margin: 0 auto;
  }
  li.Flags > :global(svg:first-child) {
    width: 10px;
    color: var(--action);
    opacity: 0.6;
  }
  p.FlagName {
    text-transform: capitalize;
  }
</style>

<li class="Children Flags">
  <IcoFlag />
  <p>Flags</p>
  <p class="Preview">
    ({!params.filterFlags ? 'All' : !Array.isArray(params['filterFlags']) ? 8 : 9 - params.filterFlags.length})
  </p>
  <ArrowDropDown />
  <ul class="FilterFlags">
    <li
      class:Current={!params.filterFlags}
      on:click={() => {
        params.filterFlags ? addArrParams('filterFlags', undefined) : '';
      }}>
      <IcoChecked />
      <p>All flags</p>
    </li>
    <span class="Division Full" />
    {#each flags as flag}
      <li
        on:click={() => {
          addArrParams('filterFlags', flag);
        }}>
        <span
          class="Checkbox"
          class:Unchecked={(params['filterFlags'] && !Array.isArray(params['filterFlags']) && params['filterFlags'] === flag) || (params['filterFlags'] && Array.isArray(params['filterFlags']) && params['filterFlags'].find((item) => item === flag))} />
        <p class="FlagName">
          {flag === 'todo' ? 'To do' : flag === 'importantterm' ? 'Important term' : flag === 'furtherreading' ? 'Further reading' : flag}
        </p>
        <svelte:component this={assignIco(flag)} />
      </li>
    {/each}
  </ul>
</li>
