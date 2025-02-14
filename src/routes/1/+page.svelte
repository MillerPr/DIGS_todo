<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  let todos = $state([
    {text: 'Learn Svelte', done: true},
    {text: 'Make a To Do List', done: false}
  ])

  $inspect(todos);

  function addTask(event: KeyboardEvent) {
    if (event.key !== 'Enter') return;
    const input = event.currentTarget as HTMLInputElement;
    const text = input.value.trim();
    const done = false;
    todos = [...todos, {text, done}];
    input.value = '';
  }
</script>

<h1>DIGS To Do App -- First Step</h1>
<div class="flex items-center space-x-2 max-w-xs py-1">
  <Input placeholder="Add a new task..." onkeydown={addTask}/>
</div>
{#each todos as todo}
  <div class="flex items-center space-x-2 max-w-xs py-1">
    <Checkbox bind:checked={todo.done} />
    <Input bind:value={todo.text} />
  </div>
{/each}
