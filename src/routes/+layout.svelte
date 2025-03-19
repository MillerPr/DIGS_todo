<script lang="ts">
	import '../app.css';
	import * as Sidebar from "$lib/components/ui/sidebar/index.js";
  import AppSidebar from "$lib/components/app-sidebar.svelte";
	// Supports weights 100-900
	import '@fontsource-variable/geist';
	let { children } = $props();
</script>

<Sidebar.Provider>
  <AppSidebar />
  <main>
    <div class="px-4">
      <Sidebar.Trigger />
      {@render children?.()}
    </div>
  </main>
</Sidebar.Provider>
