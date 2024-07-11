<script>
    import { goto } from '$app/navigation';

    let folders = [];
    let folderName = '';
    let flashCardName = '';
    let selectedFolder = null;

    function addFolder() {
        if (folderName.trim() !== '') {
            folders = [...folders, { name: folderName, flashCards: [] }];
            folderName = '';
        }
    }

    function addFlashCard() {
        if (flashCardName.trim() !== '' && selectedFolder !== null) {
            selectedFolder.flashCards = [...selectedFolder.flashCards, flashCardName];
            flashCardName = '';
        }
    }

    function selectFolder(folder) {
        selectedFolder = folder;
    }

    function navigateToCreate() {
        goto('/create');
    }
</script>

<style>
    /* Your styles remain unchanged */
</style>

<div>
    <p>Create a New Folder</p>
    <input type="text" bind:value={folderName} placeholder="Folder name" />
    <button on:click={addFolder}>Add Folder</button>

    <p>Folders</p>
    <ul>
        {#each folders as folder}
            <li>
                <div class="folder {selectedFolder === folder ? 'selected' : ''}" on:click={() =>selectFolder(folder)}>
                    {folder.name}
                </div>
                {#if selectedFolder === folder}
                    <div class="flash-card">
                        <input type="text" bind:value={flashCardName} placeholder="Flash Card name" />
                        <button on:click={addFlashCard}>Add Flash Card</button>
                    </div>
                    <ul>
                        {#each folder.flashCards as flashCard}
                            <li>{flashCard}</li>
                        {/each}
                    </ul>
                {/if}
            </li>
        {/each}
    </ul>
    <button on:click={navigateToCreate}>Create Flashcard</button>
</div>


