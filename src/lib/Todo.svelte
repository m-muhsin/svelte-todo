<script>
    let placeholderItems = [
        { id: 1, content: 'apple', done: false },
        { id: 2, content: 'orange', done: true },
        { id: 3, content: 'grapes', done: false },
    ];
    let placeholderCurrentId = 3;

    let items = [];
    let currentId = 0;

    // Fetch from localSorage and assigne to variables.
    let localItems = localStorage.getItem('svelte-todo-items');
    let localId = localStorage.getItem('svelte-todo-current-id');

    if (localId && localItems && Array.isArray(JSON.parse(localItems))) {
        items = JSON.parse(localItems)
        currentId = Number(localId)
    } else {
        items = placeholderItems;
        currentId = placeholderCurrentId;
    }

    // Reactivity: save in localStorage when these variables are modified.
    $: items, currentId && saveInLocalStorage();

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

    const rmeoveDone = () => {
        items = items.filter(item => !item.done)
    }
</script>

<form on:submit|preventDefault={formSubmit}>
    <input class="input-item" placeholder="Enter Item" />
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

<button on:click={rmeoveDone}>Remove Done</button>

<style>
    .input-item {
        height: 22px;
        vertical-align: bottom;
    }

    button {
        font-size: 1.125rem;
    }

    ul {
        list-style: none;
        padding-left: 0;
    }

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
