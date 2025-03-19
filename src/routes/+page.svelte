<script lang="ts">
  import OrderedList from "$lib/components/ordered-list/ordered-list.svelte";
  let todoItems = [
  'Add a sidebar',
  'Tweak the layout',
  'Create routes',
  'Add items to a to do list'
  ]

</script>

<h1>DIGS To Do App</h1>

<div class="container">
  <OrderedList items={todoItems} listStyle='circle' />
</div>

