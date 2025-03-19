<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import Navbar from "$lib/components/xnavbar/navbar.svelte";

// Create a store to hold the todos and provide two sample todos
  let todos = $state<Todo[]>([])
  let filter = $state<Filters>('all')
  const filterVals = ['all', 'active', 'completed']
  type Filters = (typeof filterVals)[number];
  type Todo = {text: string; done: boolean};

  $inspect(todos);
  $effect(() => {
    const savedTodos = localStorage.getItem('todos');
    savedTodos && (todos = JSON.parse(savedTodos))
  })

  $effect(() => {
    localStorage.setItem('todos', JSON.stringify(todos));
  })

  function addTask(event: KeyboardEvent) {
    if (event.key !== 'Enter') return;
    const input = event.currentTarget as HTMLInputElement;
    const text = input.value.trim();
    const done = false;
    todos = [...todos, {text, done}];
    input.value = '';
  }

  function setFilter(newFilter: Filters){
    filter = newFilter;
  }

  function filteredList(){
    return filter === 'all' ? todos :
    filter === 'active' ? todos.filter(todo => !todo.done) :
    todos.filter(todo => todo.done);
  }

  let taskCount = $derived(
    todos.filter(todo => !todo.done).length === 0 ? 'All clear' : todos.filter(todo => !todo.done).length
);

  let fCount = $derived(
    filteredList().length === 0 ? 'All clear!' : filteredList().length
  );

</script>



<div>
  <Navbar />
</div>
<h1>DIGS To Do App -- Fifth Step</h1>
<div class="filters py-4 space-x-4">
  {#each filterVals as f}
  <Button variant="outline" onclick={() => setFilter(f as Filters)}>{f}</Button>
  {/each}
</div>
<Button variant="destructive" onclick={() => todos = []}>Clear all</Button>
<div class="py-4">
  Number of incomplete tasks: {taskCount}
</div>
<div class="font-bold py-2">
  Count {filter} tasks: {fCount}
</div>
<div class="flex items-center space-x-2 max-w-xs py-1">
  <Input placeholder="Add a new task..." onkeydown={addTask}/>
</div>
{#each filteredList() as todo, i}
  <div class="flex items-center space-x-2 max-w-xs py-1">
    <Checkbox bind:checked={todo.done} />
    <Input bind:value={todo.text} />
    <Button variant="destructive" onclick={() => todos = todos.filter((_, index) => index!==i)}>x</Button>
  </div>
{/each}
