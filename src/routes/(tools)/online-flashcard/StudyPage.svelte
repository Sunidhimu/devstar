<script>
  import { onMount } from 'svelte';
  import { goto } from '@sveltejs/kit';

  let cards;
  let currentSide = "front";
  let currentCard = 0;
  let title;

  function loadFlashCards() {
    let term = cards[currentCard].term;
    let definition = cards[currentCard].definition;
    front.textContent = term;
    back.textContent = definition;
    count.textContent = currentCard + 1 + '/';
    total.textContent = cards.length;
  }

  function flip() {
    currentSide = currentSide === 'front' ? 'back' : 'front';
  }

  function reset() {
    currentCard = 0;
    currentSide = "front";
    loadFlashCards();
  }

  function goBack() {
    if (currentCard > 0) {
      currentCard -= 1;
      if (currentSide === "back") {
        flip();
      }
      loadFlashCards();
    }
  }

  function goForward() {
    if (currentCard < cards.length - 1) {
      currentCard += 1;
      if (currentSide === "back") {
        flip();
      }
      loadFlashCards();
    }
  }

  function shuffle(array) {
    let currentIndex = array.length, randomIndex;
    while (currentIndex != 0) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex--;
      [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
    }
    return array;
  }

  onMount(() => {
    cards = JSON.parse(localStorage.getItem('cards'));
    title = localStorage.getItem('currentTitle');
    loadFlashCards();
  });
</script>

<style>
  /* Add your CSS here */
</style>

<div>
  <header>
    <div class="container">
      <div class="main">Flashcard</div>
      <div class="left">
        <a href="/home" class="icon-link">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-collection" viewBox="0 0 16 16">
              <path d="M2.5 3.5a.5.5 0 0 1 0-1h11a.5.5 0 0 1 0 1zm2-2a.5.5 0 0 1 0-1h7a.5.5 0 0 1 0 1zM0 13a1.5 1.5 0 0 0 1.5 1.5h13A1.5 1.5 0 0 0 16 13V6a1.5 1.5 0 0 0-1.5-1.5h-13A1.5 1.5 0 0 0 0 6zm1.5.5A.5.5 0 0 1 1 13V6a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-.5.5z" />
            </svg>
          </div>
        </a>
        <a href="/create" class="icon-link">
          <div>
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-file-plus" viewBox="0 0 16 16">
              <path d="M8.5 6a.5.5 0 0 0-1 0v1.5H6a.5.5 0 0 0 0 1h1.5V10a.5.5 0 0 0 1 0V8.5H10a.5.5 0 0 0 0-1H8.5z" />
              <path d="M2 2a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2zm10-1H4a1 1 0 0 0-1 1v12a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1" />
            </svg>
          </div>
        </a>
        <div>
          <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" class="bi bi-person-square" viewBox="0 0 16 16">
            <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0" />
            <path d="M2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2zm12 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1v-1c0-1-1-4-6-4s-6 3-6 4v1a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1z" />
          </svg>
        </div>
      </div>
    </div>
  </header>
</div>
<hr>
<div class="container" id="FCcontent">
  <h1 id="headerText">{title}</h1>
  <div id="FC-container">
    <div id="FC" on:click={flip}>
      <div id="front">{currentCard >= 0 && cards[currentCard].term}</div>
      <div id="back">{currentCard >= 0 && cards[currentCard].definition}</div>
    </div>
  </div>
  <div class="btn-group">
    <button class="btn" on:click={goBack}>Previous</button>
    <button class="btn" on:click={reset}>Restart</button>
    <button class="btn" on:click={goForward}>Next</button>
  </div>
  <div id="bottomText">
    <span id="count">{currentCard + 1}/</span><span id="total">{cards.length}</span>
  </div>
</div>
