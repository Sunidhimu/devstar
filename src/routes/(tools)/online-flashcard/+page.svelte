<script>
	let title = 'ABC';
	let description = '123';
	let image = '';

	function handleImageChange(event) {
		const file = event.target.files[0];
		if (file) {
			const reader = new FileReader();
			reader.onload = (e) => {
				image = e.target.result;
			};
			reader.readAsDataURL(file);
		}
	}
</script>

<div class="card gap-16 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-2 overflow-hidden rounded-lg"></div>

<div class="max-w-md mx-auto p-4">
	<h1 class="text-xl font-bold mb-4">Add Flashcard</h1>
	<input type="text" class="block w-full p-2 mb-4 border-gray-300 rounded" placeholder="title" bind:value={title} />
	<textarea placeholder="description" class="block w-full p-2 mb-4 border-gray-300 rounded" bind:value={description}></textarea>
	<input type="file" accept="image/*" on:change={handleImageChange} />
	<button type="submit" class="bg-gray-900 text-white px-4 py-2 rounded hover:bg-blue-600">Save</button>
</div>

<div class="max-w-md mx-auto p-4">
	<h1 class="text-xl font-bold mb-4">Flashcards</h1>
	<div class="p-4 mb-4 border border-gray-400 rounded shadow">
		<h3 class="font-bold mb-2">{title}</h3>
		<p class="mb-2">{description}</p>
		{#if image}
			<img src={image} alt={title} class="w-full h-auto rounded" />
		{/if}
	</div>
</div>

<style>
	body{
		margin: 0;
		padding: 0;
	}
	
</style>
