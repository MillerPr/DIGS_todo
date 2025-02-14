<script lang="ts">
  import { Leaf, House, Lightbulb, Palette, Pyramid, Star } from "lucide-svelte";
  import * as Sidebar from "$lib/components/ui/sidebar/index.js";

 // Menu items.
 const items = [
  {
   title: "Home",
   url: "/",
   icon: House,
  },
  {
   title: "First",
   url: "1",
   icon: Lightbulb,
  },
  {
   title: "Second",
   url: "2",
   icon: Leaf,
  },
  {
   title: "Third",
   url: "3",
   icon: Palette,
  },
  {
   title: "Fourth",
   url: "4",
   icon: Pyramid,
  },
  {
   title: "Fifth",
   url: "5",
   icon: Star,
  },
 ];
</script>

<Sidebar.Root>
 <Sidebar.Content>
  <Sidebar.Group>
   <Sidebar.GroupLabel>DIGS To Do App</Sidebar.GroupLabel>
   <Sidebar.GroupContent>
    <Sidebar.Menu>
     {#each items as item (item.title)}
      <Sidebar.MenuItem>
       <Sidebar.MenuButton>
        {#snippet child({ props })}
         <a href={item.url} {...props}>
          <item.icon />
          <span>{item.title}</span>
         </a>
        {/snippet}
       </Sidebar.MenuButton>
      </Sidebar.MenuItem>
     {/each}
    </Sidebar.Menu>
   </Sidebar.GroupContent>
  </Sidebar.Group>
 </Sidebar.Content>
</Sidebar.Root>
