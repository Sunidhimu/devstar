<script>
	let title = '';
	let description = '';
	let frontImage = '';
	let frontContent = '';
	let backImage = '';
	let backContent = '';

	function handleImageChange(event, isFront) {
		const file = event.target.files[0];
		if (file) {
			const reader = new FileReader();
			reader.onload = (e) => {
				if (isFront) {
					frontImage = e.target.result;
				} else {
					backImage = e.target.result;
				}
			};
			reader.readAsDataURL(file);
		}
	}
</script>

<div class="card gap-16 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-2 overflow-hidden rounded-lg"></div>

<div class="max-w-md mx-auto p-4">
	<h1 class="text-xl font-bold mb-4">Add Flashcard</h1>
	<input type="text" class="block w-full p-2 mb-4 border-gray-300 rounded" placeholder="Title" bind:value={title} />
	<textarea placeholder="Description" class="block w-full p-2 mb-4 border-gray-300 rounded" bind:value={description}></textarea>
	<input type="file" accept="image/*" on:change={(event) => handleImageChange(event, true)} class="block w-full p-2 mb-4 border-gray-300 rounded" />
	<input type="text" class="block w-full p-2 mb-4 border-gray-300 rounded" placeholder="Front Content" bind:value={frontContent} />
	<textarea placeholder="Back Content" class="block w-full p-2 mb-4 border-gray-300 rounded" bind:value={backContent}></textarea>
	<input type="file" accept="image/*" on:change={(event) => handleImageChange(event, false)} class="block w-full p-2 mb-4 border-gray-300 rounded" />
	<button type="submit" class="bg-gray-900 text-white px-4 py-2 rounded hover:bg-blue-600">Save</button>
</div>

<a href="D:\AURO\flashcards\devstar\src\routes\(tools)\online-flashcard\flashcards.svelte">
	<div class="max-w-md mx-auto p-4">
		<h1 class="text-xl font-bold mb-4">Flashcards</h1>
		<div class="p-4 mb-4 border border-gray-400 rounded shadow">
			<h3 class="font-bold mb-2">{title}</h3>
			<p class="mb-2">{description}</p>
			{#if frontImage}
				<img src={frontImage} alt={title} class="w-full h-auto rounded" />
			{/if}
		</div>
	</div>
</a>

<div class="flashcard">
	<div class="front">
		<h2>{frontContent}</h2>
	</div>
	<div class="back">
		{#if backImage}
			<img src={backImage} alt="Back Image" class="w-full h-auto rounded mb-2" />
		{/if}
		<p>{backContent}</p>
	</div>
</div>

<style>
	body {
		margin: 0;
		padding: 0;
	}

	/* Container styling */
	.flashcard {
		perspective: 1000px;
		width: 300px;
		height: 200px;
		margin: 20px auto;
		position: relative;
	}

	/* Front and back styling */
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
		font-size: 1.2em;
		transition: transform 0.6s;
		padding: 10px;
		box-sizing: border-box;
		overflow: hidden;
		text-align: center;
		flex-direction: column;
	}

	/* Front styling */
	.flashcard .front {
		background-color: #ffffff;
	}

	/* Back styling */
	.flashcard .back {
		background-color: #f8f8f8;
		transform: rotateY(180deg);
	}

	/* Flip effect */
	.flashcard:hover .front {
		transform: rotateY(180deg);
	}

	.flashcard:hover .back {
		transform: rotateY(0deg);
	}

	/* Ensure content fits within the card */
	.flashcard .front img,
	.flashcard .back img {
		max-height: 50%;
		object-fit: contain;
	}

	.flashcard .front h2,
	.flashcard .back p {
		margin: 10px 0;
		word-wrap: break-word;
		overflow: hidden;
		text-overflow: ellipsis;
		max-height: 45%;
	}
</style>
