<script>
  import "./css/reset.css"
  import "./css/style.css"

  let todoTitle = "";
  let todoId = 4;
  let currentFilter = "all"
  let todos = [
    {
      id: 1,
      title: "Finish Svelte screencast",
      isComplete: false,
      isEditing: false,
    },
    {
      id: 2,
      title: "Go shopping",
      isComplete: true,
      isEditing: false,
    },
    {
      id: 3,
      title: "Take over world",
      isComplete: false,
      isEditing: false,
    },
  ];

  const addTodo = () => {
    todos = [
      ...todos,
      {
        id: todoId,
        title: todoTitle,
        isComplete: false,
        isEditing: false,
      },
    ]

    todoTitle = ""
    todoId++
  }

  const deleteTodo = (id) => {
    todos = todos.filter(todo => todo.id != id)
  }

  $: remainingTodos = todos.filter(todo => todo.isComplete == false).length

  $: filteredTodos = currentFilter == 'all' ? todos : currentFilter == 'active' ? todos.filter(todo => !todo.isComplete) : todos.filter(todo => todo.isComplete)

  const checkAll = () => {
    todos = todos.map(todo => {
      todo.isComplete = true
      return todo
    });
  }

  const clearAll = () => {
    todos = todos.filter(todo => !todo.isComplete)
  }

</script>

<div class="todo-app-container">
  <div class="todo-app">
    <h2>Todo App</h2>
    <form on:submit|preventDefault="{addTodo}">
      <input
        type="text"
        class="todo-input"
        placeholder="What do you need to do?"
        bind:value={todoTitle}
      />
    </form>

    {#if todos.length > 0}
    <ul class="todo-list">

      {#each filteredTodos as todo}
        <li class="todo-item-container">
        <div class="todo-item">
          <input type="checkbox" bind:checked={todo.isComplete}/>
          <span 
            class="todo-item-label"
            class:line-through={todo.isComplete}
          >{todo.title}</span>
          <!-- <input
            type="text"
            class="todo-item-input"
            value="Finish Svelte Series"
          /> -->
        </div>
        <button class="x-button" on:click="{deleteTodo(todo.id)}">
          <svg
            class="x-button-icon"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              strokeLinecap="round"
              strokeLinejoin="round"
              strokeWidth={2}
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
      </li>
      {/each}
    </ul>
    <div class="check-all-container">
      <div>
        <button class="button" on:click={checkAll}>Check All</button>
      </div>
      <span>{remainingTodos} items remaining</span>
    </div>

    <div class="other-buttons-container">
      <div>
        <button on:click={() => currentFilter = 'all'} class="button filter-button" class:filter-button-active={currentFilter == 'all'}> All </button>
        <button on:click={() => currentFilter = 'active'} class="button filter-button" class:filter-button-active={currentFilter == 'active'}>Active</button>
        <button on:click={() => currentFilter = 'completed'} class="button filter-button" class:filter-button-active={currentFilter == 'completed'}>Completed</button>
      </div>
      <div>
        <button class="button" on:click="{clearAll}">Clear completed</button>
      </div>
    </div>
    {:else}
      <div class="no-todos-container">Data not found, Please create todo...</div>
    {/if}
  </div>
</div>
    