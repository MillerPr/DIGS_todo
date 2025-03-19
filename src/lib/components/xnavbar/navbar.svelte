<script lang="ts">

  let menuItems: { name: string, route: string }[] = [
    {name: 'Home', route: '/'},
    {name: 'First', route: '1'},
  ];
</script>

{#snippet nav(menuItems: { name: string, route: string }[])}
  <nav>
    <span>Menu: </span>
      {#each menuItems as link}
      <span class="px-3"><a href="{link.route}">{link.name}</a></span>
      {/each}
  </nav>
{/snippet}

{@render nav(menuItems)}
