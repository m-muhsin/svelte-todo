<script>
    let items = [
        { id: 1, content: 'apple', done: false },
        { id: 2, content: 'orange', done: true },
        { id: 3, content: 'grapes', done: false },
    ];
    let currentId = 3;

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

    const removeItem = (index) => {
        items = items.filter(item => (item.id !==index))
    }

    const onCheck = (id) => {
        const updatedItems = items.map(item => {
        if (item.id === id) {
                item.done = !item.done;
            }
            return item;
        })
        items = updatedItems;
    }
</script>

<form on:submit|preventDefault={formSubmit}>
    <input placeholder="Enter Item" />
    <button type="submit">Add Item</button>
</form>

<ul>
{#each items as {id, done, content}, index (id)}
    <li>
        <label class={done ? 'done' : ''}>
            <input type="checkbox" checked={done} on:change|preventDefault={() => onCheck(id)} />
            {content} 
        </label>
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