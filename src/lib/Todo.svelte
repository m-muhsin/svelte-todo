<script>
    let items = [
        { id: 1, content: 'apple', done: false },
        { id: 2, content: 'orange', done: true },
        { id: 3, content: 'grapes', done: false },
    ];
    let currentId = 3;

    let localItems = localStorage.getItem('svelte-todo-items');
    let localId = localStorage.getItem('svelte-todo-current-id');

    if (localItems) {
        items = JSON.parse(localItems)
    }
    if (localId) {
        currentId = Number(localId)
    }

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

        saveInLocalStorage(items, currentId);
        
        event.target.reset();
    }
    
    const saveInLocalStorage = (items, currentIdParam = currentId) => {
        localStorage.setItem('svelte-todo-items', JSON.stringify(items));
        localStorage.setItem('svelte-todo-current-id', String(currentIdParam));
    }

    const removeItem = (id) => {
        items = items.filter(item => (item.id !==id))
        saveInLocalStorage(items);
    }

    const onCheck = (id) => {
        const updatedItems = items.map(item => {
        if (item.id === id) {
                item.done = !item.done;
            }
            return item;
        })
        items = updatedItems;
        saveInLocalStorage(items);
    }
</script>

<form on:submit|preventDefault={formSubmit}>
    <input placeholder="Enter Item" />
    <button type="submit">Add Item</button>
</form>

<ul>
{#each items as {id, done, content} (id)}
    <li>
        <label class={done ? 'done' : ''}>
            <input type="checkbox" checked={done} on:change|preventDefault={() => onCheck(id)} />
        </label>
        <span contenteditable="true">{content}</span>
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