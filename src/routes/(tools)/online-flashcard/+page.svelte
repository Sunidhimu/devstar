<script>
    import { onMount } from 'svelte';

    let studySets = [];
    let currentSet = -1;

    function loadHome() {
        const numOfSets = localStorage.getItem('set#');
        if (numOfSets > 0) {
            for (let i = 1; i <= numOfSets; i++) {
                let setData = JSON.parse(localStorage.getItem(i));
                let setTitle = setData[0]['title'];
                studySets.push({ id: i, title: setTitle });
            }
        }
    }

    function deleteSet(setNumber) {
        if (currentSet === setNumber) {
            localStorage.removeItem("cards");
            localStorage.removeItem("currentTitle");
            currentSet = -1;
        }
        studySets = studySets.filter(set => set.id !== setNumber);
        localStorage.removeItem(setNumber);
        let numSets = localStorage.getItem("set#");
        for (let i = 1; i <= numSets; i++) {
            if (i > setNumber) {
                let oldKey = i;
                let newKey = i - 1;
                localStorage.setItem(newKey, localStorage.getItem(oldKey));
                localStorage.removeItem(oldKey);
                console.log("Set " + oldKey + " is now set " + newKey);
            }
        }
        localStorage.setItem('set#', numSets - 1);
    }

    function loadSetData(setNumber) {
        console.log(setNumber);
        currentSet = setNumber;
        let data = JSON.parse(localStorage.getItem(currentSet));
        let title = data[0]['title'];
        let cards = [];
        for (let i = 1; i < data.length; i++) {
            let term = Object.keys(data[i])[0];
            let definition = data[i][term];
            let kvp = {};
            kvp[term] = definition;
            cards.push(kvp);
        }
        localStorage.setItem('currentTitle', title);
        localStorage.setItem('cards', JSON.stringify(cards));
        localStorage.setItem('currentSet', setNumber);
    }

    onMount(loadHome);
</script>

<style>
    .set {
        margin-bottom: 1em;
        padding: 1em;
        border: 1px solid #ccc;
        border-radius: 5px;
    }

    .setHeader {
        display: flex;
        justify-content: space-between;
        margin-bottom: 0.5em;
    }

    .setLink {
        text-decoration: none;
        color: #007bff;
    }

    .setDelete {
        cursor: pointer;
        color: red;
    }

    #createBtn {
        display: block;
        margin-top: 1em;
    }
</style>

<h1>Your Study Sets</h1>
<div class="container" id="studySets">
    <a href="/create" id="createBtn">Create New Study Set</a>

    {#each studySets as set}
        <div class="set">
            <div class="setHeader">
                <span></span>
                <span class="setDelete" on:click={() => deleteSet(set.id)}>X</span>
            </div>
            <a href="/study" class="setLink" on:click={() => loadSetData(set.id)}>{set.title}</a>
        </div>
    {/each}
</div>
