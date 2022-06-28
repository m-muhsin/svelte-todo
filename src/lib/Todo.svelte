<script>
    let items = [
        { id: 1, content: 'apple', done: false },
        { id: 2, content: 'orange', done: true },
        { id: 3, content: 'grapes', done: false },
    ];

    const formSubmit = (event) => {
        items = [...items,event.target[0].value];
        event.target.reset();
    }

    const removeItem = (index) => {
        console.log(index)
        items = items.filter((items, i) => (i !==index))
    }

    const onCheck = (id) => {
        console.log('id',id)
        const updatedItems = items.map(item => {
            if (item.id === id) {
                let opposite = Boolean(!item.done);
                item.done = opposite;
                console.log('item.done',item.done)
                console.log('!item.done',!item.done)
            }
            return item;
        })
        items = updatedItems;
        console.log('updatedItems',updatedItems)
        console.log('items',items)
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
            <input type="checkbox" bind:checked={done} on:change={() => onCheck(id)} />
            {content} 
        </label>
        <button on:click={() => removeItem(index)} class="close">&times;</button>
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