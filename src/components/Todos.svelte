<script>
    import { slide } from "svelte/transition";
    import { cubicInOut } from "svelte/easing";
    import { todos } from "../js/stores";

    let newTodo = "";
    let itemLength;

    const addTodo = (event) => {
        newTodo = event.target.task.value;
        newTodo.trim();

        if (!newTodo) return;

        newTodo = {
            text: newTodo,
            checked: false,
            id: Math.random().toString(36).substr(2, 9),
        };

        $todos = [...$todos, newTodo];
        itemLength = $todos.length;
    };

    const toggleDone = (id) => {
        const index = $todos.findIndex((item) => item.id === id);
        $todos[index].checked = !$todos[index].checked;
    };

    const deleteTodo = (id) => {
        const index = $todos.findIndex((item) => item.id === id);
        $todos.splice(index, 1);
        $todos = $todos;
        itemLength = $todos.length;
    };
</script>

<form on:submit|preventDefault={addTodo}>
    <input
        id="task"
        class="js-todo-input"
        type="text"
        aria-label="Enter a new todo item"
        placeholder="E.g. Build a web app"
    />
</form>
<h2>Your tasks</h2>
{#if itemLength == undefined || itemLength == 0}
    <p>No Tasks Left</p>
{:else if itemLength >= 1}
    <p>{itemLength} Tasks Left</p>
{/if}
<ul class="todo-list">
    {#each $todos as todo (todo.id)}
        <div>
            <li
                transition:slide={{ duration: 600, easing: cubicInOut }}
                class="todo-item {todo.checked ? 'done' : ''}"
            >
                <input
                    id={todo.id}
                    type="checkbox"
                    on:click={() => toggleDone(todo.id)}
                />
                <label for={todo.id} class="tick" />
                <p>{todo.text}</p>
            </li>
            <button
                class="delete-todo {todo.checked ? 'checked' : ''}"
                on:click={() => deleteTodo(todo.id)}
            >
                <i class="gg-trash" />
            </button>
        </div>
    {/each}
</ul>

<style>
    ul {
        padding-inline-start: 0;
        margin-block-start: 0;
    }

    ul div {
        display: flex;

        margin: 1em 0;
    }

    ul li {
        display: flex;
        list-style-type: none;
        justify-content: start;
        align-items: center;
        background-color: #ffffff;
        padding: 0 0 0 1em;
        width: 30%;
        border-radius: 20px;
        box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.2);
        opacity: 100%;
        transition: 300ms all;
    }

    ul li p {
        padding-left: 0.5em;
    }

    .todo-list li input[type="checkbox"] {
        background-color: #eaebee;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        padding: 0.6em;
        margin: 0;
        border: none;
        transition: 200ms all;
        cursor: pointer;
        border-radius: 5px;
    }

    .todo-list li input[type="checkbox"]:checked {
        background-color: #c1c1ee;
    }

    .done {
        opacity: 50%;
        transition: 300ms all;
    }

    .done p {
        transition: 0.2s all;
        text-decoration: line-through;
    }

    button {
        background-color: transparent;
        position: relative;
        border: none;
        left: 1.2em;
        cursor: pointer;
        opacity: 0;
        transition: 600ms all;
    }

    button.checked {
        opacity: 1;
    }

    button i {
        color: #808bab;
    }
</style>
