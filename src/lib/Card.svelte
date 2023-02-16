<script>
  export let name = "";
  export let surname = "";
  export let dead = "";
  export const description = "";

  export let selected;
  export let selectedPerson;
  export let i;

  export let age;

  function selectAPerson(i) {
    selected = selected === i ? undefined : i;
  }
</script>

<div
  class="trombinoscope__item"
  on:click={() => selectAPerson(i)}
  on:keypress={() => selectAPerson(i)}
>
  <p>
    {name}
    {surname}
  </p>
  Age : {age} ans {dead ? "au moment du décès" : ""}
  {#if selectedPerson !== undefined && selectedPerson === i}
    <div>
      <p>
        Naissance : {selectedPerson.born}
      </p>
      <p>
        Description : {selectedPerson.description}
      </p>
    </div>
  {/if}
</div>

<style>
  .trombinoscope__item {
    border: 1px solid black;
    padding: 10px;
    width: 28%;
  }
</style>
