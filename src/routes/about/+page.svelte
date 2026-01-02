

<svelte:head>
	<title>Mytodolist</title>
	<meta name="description" content="About this app" />
</svelte:head>

<script lang="ts">
  let taskInput: string = "";
  let tasks: string[] = [];

  function addTask(): void {
    if (!taskInput.trim()) return;
    tasks = [...tasks, taskInput];
    taskInput = "";
  }

  function removeTask(index: number): void {
    tasks.splice(index, 1);
    tasks = [...tasks];
  }

  function handleKeypress(e: KeyboardEvent): void {
    if (e.key === "Enter") addTask();
  }
</script>

<div class="todo-container">
  <h1>My To Do List</h1>

  <input
    type="text"
    placeholder="Masukan Tugas"
    bind:value={taskInput}
    on:keypress={handleKeypress}
  />

  <button on:click={addTask}>Tambah</button>

  <ul>
    {#each tasks as task, index}
      <li>
        {task}
        <button on:click={() => removeTask(index)}>Hapus</button>
      </li>
    {/each}
  </ul>
</div>

<style>
  .todo-container {
    max-width: 600px;
    margin: 100px auto;
    padding: 20px;
    width: 250px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background: white;
  }

  h1 {
    text-align: center;
  }

  input {
    width: 100%;
    margin-bottom: 10px;
    padding: 6px;
  }

  button {
    width: 100%;
    background-color: rgb(3, 137, 182);
    color: white;
    cursor: pointer;
    border: none;
    padding: 6px;
  }

  button:hover {
    background-color: rgb(1, 69, 117);
  }

  ul {
    list-style: none;
    padding: 0;
    margin-top: 10px;
  }

  li {
    display: flex;
    align-items: center;
    background-color: #ccc;
    padding: 10px;
    margin-bottom: 10px;
  }

  li button {
    margin-left: auto;
    width: auto;
    background: red;
  }
</style>
