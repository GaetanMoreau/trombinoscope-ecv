<script>
    export let name = "";
    export let surname = "";
    export let born = "";
    export let dead = "";
    export const description = "";

    export let selected;
    export let selectedPerson;
    export let i;

    export let searchedName;
    export let searchedAge;

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

    let age = getAgeOfPerson(born, dead);

    function selectAPerson(i) {
        selected = selected === i ? undefined : i;
    }
</script>

{#if surname
    .toLowerCase()
    .includes(searchedName.toLowerCase()) && age < searchedAge}
    <div
        class="trombinoscope__item"
        on:click={() => selectAPerson(i)}
        on:keypress={() => selectAPerson(i)}
    >
        <p>
            {name}
            {surname}
        </p>
        Age : {getAgeOfPerson(born, dead)} ans {dead
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

<style>
    .trombinoscope__item {
        border: 1px solid black;
        padding: 10px;
        width: 28%;
    }
</style>
