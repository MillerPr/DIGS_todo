<script lang="ts">
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
// Create a store to hold the todos and provide two sample todos
  type Todo = {text: string; done: boolean};
  let todos = $state<Todo[]>([])
  type Filters = (typeof filterVals)[number];
  let filter = $state<Filters>('all')
  const filterVals = ['all', 'active', 'completed']

  $inspect(todos);

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

  function taskCount(){
    return todos.filter(todo => !todo.done).length === 0 ? 'All clear' : todos.filter(todo => !todo.done).length;
  }
</script>

<h1>DIGS To Do App -- Second Step</h1>
<div class="filters py-4 space-x-4">
  {#each filterVals as f}
  <Button variant="outline" onclick={() => setFilter(f as Filters)}>{f}</Button>
  {/each}
</div>
<div class="py-4">
  Number of incomplete tasks: {taskCount()}
</div>
<div class="flex items-center space-x-2 max-w-xs py-1">
  <Input placeholder="Add a new task..." onkeydown={addTask}/>
</div>
{#each filteredList() as todo}
  <div class="flex items-center space-x-2 max-w-xs py-1">
    <Checkbox bind:checked={todo.done} />
    <Input bind:value={todo.text} />
  </div>
{/each}
