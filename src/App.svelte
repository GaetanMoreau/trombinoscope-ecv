<script>
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

  function getAgeOfPerson(personBirth, personDeath) {
    let todayDate = new Date();
    let dateOfBirth = new Date(personBirth);
    let dateDiff;
    if (personDeath) {
      let dateOfDeath = new Date(personDeath);
      dateDiff = dateOfDeath - dateOfBirth;
    } else {
      dateDiff = todayDate - dateOfBirth;
    }
    let diffInYears = Math.floor(dateDiff / (1000 * 60 * 60 * 24 * 365.25));
    return diffInYears;
  }

  let selected;
  $: selectedPerson = persons[selected];

  function selectAPerson(i) {
    selected = selected === i ? undefined : i;
  }

  let displayAllPersons = true;
  function filterPersonsByStatus() {
    displayAllPersons = !displayAllPersons;
  }
  $: personToDisplay = displayAllPersons
    ? persons
    : persons.filter((t) => !t.dead);

  let searchedName = "";
  function searchByName(e) {
    searchedName = e.target.value;
  }

  let searchedAge = "";
</script>

<h1>Trombinoscope</h1>
<div class="trombinoscope__filters">
  <button on:click={filterPersonsByStatus}>
    {displayAllPersons ? "Uniquement vivants" : "Afficher tous"}
  </button>
  <input
    type="text"
    placeholder="Recherche par nom"
    on:input={(e) => searchByName(e)}
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
  {#each personToDisplay as person, i}
    {#if person.surname.toLowerCase().includes(searchedName.toLowerCase())}
      <div class="trombinoscope__item" on:click={() => selectAPerson(i)}>
        <p>
          {person.name}
          {person.surname}
        </p>
        Age : {getAgeOfPerson(person.born, person.dead)} ans {person.dead
          ? "au moment du décès"
          : ""}
        {#if selected !== undefined && selected === i}
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
    {/if}
  {/each}
</section>

<style>
  .trombinoscope__container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }
  .trombinoscope__item {
    border: 1px solid black;
    padding: 10px;
    width: 28%;
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
