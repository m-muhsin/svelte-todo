<script>
    let items = [
        { id: 1, content: 'apple', done: false },
        { id: 2, content: 'orange', done: true },
        { id: 3, content: 'grapes', done: false },
    ];
    let currentId = 3;

    // Reactivity: save in localStorage when these variables are modified.
    $: items, currentId && saveInLocalStorage();

    // Fetch from localSorage and assigne to variables.
    let localItems = localStorage.getItem('svelte-todo-items');
    let localId = localStorage.getItem('svelte-todo-current-id');
    if (localId && localItems && Array.isArray(localItems)) {
        items = JSON.parse(localItems)
        currentId = Number(localId)
    }

    // When the item is sent to be saved.
    const formSubmit = (event) => {
        if (!event.target[0].value) {
            return
        }
        items = [{
                id: ++currentId,
                content: event.target[0].value,
                done: false
            },
            ...items,
        ];        
        event.target.reset();
    }
    
    const saveInLocalStorage = (itemsParam = items, currentIdParam = currentId) => {
        localStorage.setItem('svelte-todo-items', JSON.stringify(itemsParam));
        localStorage.setItem('svelte-todo-current-id', String(currentIdParam));
    }

    const removeItem = (id) => {
        items = items.filter(item => (item.id !==id))
    }
</script>

<form on:submit|preventDefault={formSubmit}>
    <input placeholder="Enter Item" />
    <button type="submit">Add Item</button>
</form>

<ul>
{#each items as {id, done, content} (id)}
    <li>
        <label class:done={done}>
            <input type="checkbox" bind:checked={done} />
        </label>
        <span bind:innerHTML={content} contenteditable="true">{content}</span>
        <button on:click={() => removeItem(id)} class="close">&times;</button>
    </li>
{/each}
</ul>


<style>
    .close {
        background: unset;
        border: none;
        color: red;
        font-size: 2em;
        vertical-align: middle;
    }

    .done {
        text-decoration: line-through;
    }
</style>