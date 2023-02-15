<script>
  import Card from "./lib/Card.svelte";

  const persons = [
    {
      name: "Romain",
      surname: "l'Ourson",
      born: "1985-12-31",
      description: "Someone who loves Svelte !",
    },
    {
      name: "Harry",
      surname: "Potter",
      born: "1980-07-31",
      description: "A wizard who defeated He-who-must-not-be-named",
    },
    {
      name: "Steve",
      surname: "Jobs",
      born: "1955-02-24",
      dead: "2011-10-05",
      description: "Someone who loved apples",
    },
    {
      name: "Diego",
      surname: "Maradona",
      born: "1960-10-30",
      dead: "2020-11-25",
      description: "The hand of God",
    },
    {
      name: "Osamu",
      surname: "Tezuka",
      born: "1928-11-03",
      dead: "1989-02-09",
      description: "Astro boy's dad",
    },
    {
      name: "Billie",
      surname: "Eilish",
      born: "2001-12-18",
      description: "Happier then ever",
    },
    {
      name: "Bernard",
      surname: "Minet",
      born: "1953-12-28",
      description: "Un mec musclé",
    },
  ];

  let selected;
  $: selectedPerson = persons[selected];

  function getAgeOfPerson(personBirth, personDeath) {
    let todayDate = new Date();
    let dateOfBirth = new Date(personBirth);
    let dateDiff;
    if (personDeath) {
      let dateOfDeath = new Date(personDeath);
      dateDiff = dateOfDeath.getTime() - dateOfBirth.getTime();
    } else {
      dateDiff = todayDate.getTime() - dateOfBirth.getTime();
    }
    let diffInYears = Math.floor(dateDiff / (1000 * 60 * 60 * 24 * 365.25));
    return diffInYears;
  }

  let displayAllPersons = true;
  function filterPersonsByStatus() {
    displayAllPersons = !displayAllPersons;
  }
  $: personsToDisplay = displayAllPersons
    ? persons
    : persons.filter((person) => !person.dead);

  $: personToDisplaySearch = personsToDisplay.filter((person) =>
    person.surname.toLowerCase().includes(searchedName.toLowerCase())
  );

  $: personToDisplayAge = personToDisplaySearch.filter(
    (person) => getAgeOfPerson(person.born, person.dead) < searchedAge
  );

  let searchedName = "";
  function searchPersonsByName(e) {
    searchedName = e.target.value;
  }
  function backToList() {
    selectedPerson = undefined;
  }
  let searchedAge = 100;
</script>

<h1>Trombinoscope</h1>

{#if selectedPerson !== undefined}
  <div>
    <p>Prénom : {selectedPerson.name}</p>
    <p>Nom : {selectedPerson.surname}</p>
    <p>Naissance : {selectedPerson.born}</p>
    {#if selectedPerson.dead}<p>Mort : {selectedPerson.dead}</p> {/if}
    <p>Description : {selectedPerson.description}</p>
  </div>
  <button on:click={backToList}>Retour</button>
{:else}
  <div class="trombinoscope__filters">
    <button on:click={filterPersonsByStatus}>
      {displayAllPersons ? "Uniquement vivants" : "Afficher tous"}
    </button>

    <input
      type="text"
      placeholder="Recherche par nom"
      on:input={(e) => searchPersonsByName(e)}
      bind:value={searchedName}
    />

    <div class="filterByAge">
      <label for="age">Recherche par age</label>
      <input
        type="range"
        min="0"
        max="100"
        step="1"
        name="age"
        bind:value={searchedAge}
      />
      <p>Age: {searchedAge}</p>
    </div>
  </div>
  <section class="trombinoscope__container">
    {#each personToDisplayAge as person, i}
      <Card
        {...person}
        bind:selected
        {selectedPerson}
        {i}
        age={getAgeOfPerson(person.born, person.dead)}
      />
    {/each}
  </section>
{/if}

<style>
  .trombinoscope__container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }
  .trombinoscope__filters {
    display: flex;
    gap: 1rem;
    margin-bottom: 2rem;
  }
  .filterByAge {
    display: flex;
    gap: 1rem;
    align-items: center;
  }
</style>
