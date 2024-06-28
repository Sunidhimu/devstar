<script>
	let flashcards = [];
	let title = '';
	let description = '';
	let frontImage = '';
	let frontContent = '';
	let backImage = '';
	let backContent = '';
	let isEditing = false;
	let editIndex = null;

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

	function addOrUpdateFlashcard() {
		const newFlashcard = {
			title,
			description,
			frontImage,
			frontContent,
			backImage,
			backContent
		};

		if (isEditing && editIndex !== null) {
			flashcards[editIndex] = newFlashcard;
			isEditing = false;
			editIndex = null;
		} else {
			flashcards = [...flashcards, newFlashcard];
		}

		title = '';
		description = '';
		frontImage = '';
		frontContent = '';
		backImage = '';
		backContent = '';
	}

	function editFlashcard(index) {
		const flashcard = flashcards[index];
		title = flashcard.title;
		description = flashcard.description;
		frontImage = flashcard.frontImage;
		frontContent = flashcard.frontContent;
		backImage = flashcard.backImage;
		backContent = flashcard.backContent;
		isEditing = true;
		editIndex = index;
	}
</script>

<div class="max-w-md mx-auto p-4">
	<h1 class="text-xl font-bold mb-4">{isEditing ? 'Edit Flashcard' : 'Add Flashcard'}</h1>
	<input type="text" class="block w-full p-2 mb-4 border-gray-300 rounded" placeholder="Title" bind:value={title} />
	<textarea placeholder="Description" class="block w-full p-2 mb-4 border-gray-300 rounded" bind:value={description}></textarea>
	<input type="file" accept="image/*" on:change={(event) => handleImageChange(event, true)} class="block w-full p-2 mb-4 border-gray-300 rounded" />
	<input type="text" class="block w-full p-2 mb-4 border-gray-300 rounded" placeholder="Front Content" bind:value={frontContent} />
	<textarea placeholder="Back Content" class="block w-full p-2 mb-4 border-gray-300 rounded" bind:value={backContent}></textarea>
	<input type="file" accept="image/*" on:change={(event) => handleImageChange(event, false)} class="block w-full p-2 mb-4 border-gray-300 rounded" />
	<button type="button" class="bg-gray-900 text-white px-4 py-2 rounded hover:bg-blue-600" on:click={addOrUpdateFlashcard}>{isEditing ? 'Update Flashcard' : 'Add Flashcard'}</button>
</div>

<div class="flashcard-container">
	{#each flashcards as flashcard, index}
		<div class="flashcard-wrapper">
			<div class="flashcard">
				<div class="front">
					{#if flashcard.frontImage}
						<img src={flashcard.frontImage} alt="Front Image" class="w-full h-auto rounded mb-2" />
					{/if}
					<h2>{flashcard.frontContent}</h2>
				</div>
				<div class="back">
					{#if flashcard.backImage}
						<img src={flashcard.backImage} alt="Back Image" class="w-full h-auto rounded mb-2" />
					{/if}
					<p>{flashcard.backContent}</p>
				</div>
			</div>
			<button type="button" class="mt-2 px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-800" on:click={() => editFlashcard(index)}>Edit</button>
		</div>
	{/each}
</div>

<style>
	body {
		margin: 0;
		padding: 0;
	}

	/* Container styling */
	.flashcard-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}

	.flashcard-wrapper {
		margin: 10px;
	}

	.flashcard {
		perspective: 1000px;
		width: 300px;
		height: 200px;
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
