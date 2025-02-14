<script lang="ts">
  let todoItems = ['Add a sidebar', 'Tweak the layout', 'Create routes', 'Add items to a to do list']
  import {OrderedList} from "$lib/components/ui/ordered-list";
</script>

<h1>DIGS To Do App</h1>

<OrderedList items={todoItems} listStyle='circle' />
