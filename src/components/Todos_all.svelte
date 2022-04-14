<script>
    export let todos = []
    let newTodoName = ''
    // let totalTodos = todos.length
    // let completedTodos = todos.filter(todo => todo.completed).length
    // reactive by prefixing with $ :
    $: totalTodos = todos.length
    $: completedTodos = todos.filter(todo => todo.completed).length

    $: newTodoId = totalTodos ? Math.max(...todos.map(t => t.id)) + 1 : 1

    const removeTodo = (todo) => {
        todos = todos.filter(t => t.id !== todo.id)
    }

    const addTodo = () => {
        todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }]
        newTodoName = ''
    }

    let filter = 'all'
    const filterTodos = (filter, todos) =>
        filter === 'active' ? todos.filter(t => !t.completed) :
        filter === 'completed' ? todos.filter(t => t.completed) :
        todos
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">

    <!-- NewTodo -->
    <form on:submit|preventDefault={addTodo}>
        <h2 class="label-wrapper">
            <label for="todo-0" class="label__lg">
                What needs to be done?
            </label>
        </h2>
        <input type="text" id="todo-0" autocomplete="off"
        class="input input__lg" bind:value={newTodoName} />
        <button type="submit" disabled="" class="btn btn__primary btn__lg">
            Add
        </button>
    </form>

    <!-- Filter -->
    <div class="filters btn-group stack-exception">
        <!--  aria-pressed="true" 按鈕預設是被按下 -->
        <button class="btn toggle-btn" class:btn__primary={filter === 'all'} aria-pressed={filter === 'all'} on:click={()=> filter = 'all'} >
            <span class="visually-hidden">Show</span>
            <span>All</span>
            <span class="visually-hidden">tasks</span>
        </button>
        <button class="btn toggle-btn" class:btn__primary={filter === 'active'} aria-pressed={filter === 'active'} on:click={()=> filter = 'active'} >
            <span class="visually-hidden">Show</span>
            <span>Active</span>
            <span class="visually-hidden">tasks</span>
        </button>
        <button class="btn toggle-btn" class:btn__primary={filter === 'completed'} aria-pressed={filter === 'completed'} on:click={()=> filter = 'completed'} >
            <span class="visually-hidden">Show</span>
            <span>Completed</span>
            <span class="visually-hidden">tasks</span>
        </button>
    </div>

    <!-- TodosStatus -->
    <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>

    <!-- Todos -->
    <!-- role屬性解釋元素具有什麼種類的語義數值 aria-labelledby把<h2>且id為list-heading作描述下面清單用途的標籤-->
    <!-- svelte-ignore a11y-no-redundant-roles -->
    <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
        {#each filterTodos(filter, todos) as todo (todo.id)}
        <li class="todo">
            <div class="stack-small">
            <div class="c-cb">
                <input type="checkbox" id="todo-{todo.id}"
                on:click={() => todo.completed = !todo.completed}
                checked={todo.completed}
                />
                <label for="todo-{todo.id}" class="todo-label">
                    {todo.name}
                </label>
            </div>
            <div class="btn-group">
                <button type="button" class="btn">
                Edit <span class="visually-hidden">{todo.name}</span>
                </button>
                <button type="button" class="btn btn__danger"
                on:click={() => removeTodo(todo)}
                >
                Delete <span class="visually-hidden">{todo.name}</span>
                </button>
            </div>
            </div>
        </li>
        {:else}
        <li>Nothing to do here!</li>
        {/each}
    </ul>

    <hr />

    <!-- MoreActions -->
    <div class="btn-group">
        <button type="button" class="btn btn__primary">Check all</button>
        <button type="button" class="btn btn__primary">Remove completed</button>
    </div>

</div>