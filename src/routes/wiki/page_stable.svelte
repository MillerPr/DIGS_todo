<script lang="ts">
  import { Input } from "$lib/components/ui/input/index.js";
  import { ExternalLink } from "lucide-svelte";

  //Create a store for search results pages
  let pages = $state<Page[]>([])
  $inspect(pages)
  type Page = {
    index: number;
    ns: number;
    pageid: number;
    title: string;
  }

  let searchTerm = $state('')

  let loading = false;
  let error: string | null = null;

  async function searchWiki(event: KeyboardEvent) {
    loading = true;
    error = null;
    // Get input
    if (event.key !== 'Enter') return;
    const input = event.currentTarget as HTMLInputElement;
    searchTerm = input.value.trim();
    // Construct URL
    const params: any = {
      action: "query",
      format: "json",
      generator: "search",
      gsrnamespace: "0",
      gsrlimit: "20",
      gsrsearch: searchTerm
    };
    let url = "https://en.wikipedia.org/w/api.php";
    url = url + "?origin=*";
    const queryString = new URLSearchParams(params).toString();
    url = `${url}&${queryString}`;
    //console.log(url)

    // Fetch
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const data = await response.json();
      if (data.query && data.query.pages) {
        pages = Object.values(data.query.pages).map((page: any) => ({
          index: page.index,
          pageid: page.pageid,
          title: page.title,
          ns: page.ns,
        }));
      } else {
        throw new Error('Invalid response format');
      }
    } catch (e) {
      if (e instanceof Error) {
        error = e.message;
      } else {
        error = 'An unknown error occurred.';
      }
    } finally {
      loading = false;
    }
  }

</script>

<h1>Search Wikipedia</h1>
<div class="flex items-center space-x-2 max-w-xs py-1">
  <Input placeholder="Search Wikipedia..." onkeydown={searchWiki}/>
</div>
{#each pages as page, i}
<div class="flex items-center space-x-2 py-1">
  <a href="https://www.wikipedia.org/{page.pageid}" target="_blank">{page.title}</a><ExternalLink />
</div>
{/each}
