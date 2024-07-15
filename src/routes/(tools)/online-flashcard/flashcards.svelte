<script>
	import { flashcards } from './+page.svelte';
  
	let cards = [];
  
	flashcards.subscribe(value => {
	  cards = value;
	});
  </script>
  
  <div>
	<h1 class="text-xl font-bold mb-4 bg-white p-2">Flashcards</h1>
	{#each cards as card}
	  <div class="p-4 mb-4 border border-gray-400 rounded shadow">
		<h1 class="font-black mb-2 bg-white p-2 uppercase underline text-8xl ">{card.title}</h1>
		<p class="mb-2 bg-white p-2 font-normal lowercase text-2xl " >{card.description}</p>
		{#if card.frontImage}
		  <img src={card.frontImage} alt={card.title} class="w-full h-auto rounded" />
		{/if}
		<div class="flashcard">
		  <div class="front">
			<h1>{card.frontContent}</h1>
		  </div>
		  <div class="back">
			{#if card.backImage}
			  <img src={card.backImage} alt="Back Image" class="w-full h-auto rounded mb-2" />
			{/if}
			<p>{card.backContent}</p>
		  </div>
		</div>
	  </div>
	{/each}
  </div>
  
  <style>
	body {
	  margin: 0;
	  padding: 0;
	}
  
  
	.flashcard {
	  perspective: 1000px;
	  width: 1000px;
	  height: 600px;
	  margin: 20px auto;
	  position: relative;
	}
  
	.flashcard .front,
	.flashcard .back {
	  width: 100%;
	  height: 100%;
	  position: absolute;
	  backface-visibility: hidden;
	  border: 1px solid #ccc;
	  border-radius: 10px;
	  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  font-family: Arial, sans-serif;
	  font-size: 60px;
	  transition: transform 0.6s;
	  padding: 10px;
	  box-sizing: border-box;
	  overflow: hidden;
	  text-align: center;
	  flex-direction: column;
	}
  
	.flashcard .front {
	  background-color: #ffffff;
	}
  
	.flashcard .back {
	  background-color: #f8f8f8;
	  transform: rotateY(180deg);
	}
  
	.flashcard:hover .front {
	  transform: rotateY(180deg);
	}
  
	.flashcard:hover .back {
	  transform: rotateY(0deg);
	}
  
	.flashcard .front img,
	.flashcard .back img {
	padding-top: 20px;
	padding-bottom: 20px;
	padding-left: 20px;
	padding-right: 20px;
	height: 1000px;
	width:900px
	object-fit: contain;
	}
  
	.flashcard .front h1,
	.flashcard .back p {
	  margin: 10px 0;
	  word-wrap: break-word;
	  overflow: hidden;
	  text-overflow: ellipsis;
	  text font: 20px;
	  max-height: 45%;
	  text-decoration: solid;
	  font-size: medium;
	  
	}
  </style>
  