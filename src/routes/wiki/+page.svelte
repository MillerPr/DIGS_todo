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
    source: string;
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

    // Fetch, is there a better way to do this?
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
          source: ''
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

  // Can this be $derived?
  // Fix logic to handle results without valid images.
  async function wikiImage(pageid: string){
    let sourceString: string
    let url: string = `https://en.wikipedia.org/w/api.php?origin=*&action=query&prop=pageimages&pageids=${pageid}&pithumbsize=100&format=json`
    // Fetch
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const data = await response.json();
      if (data.query.pages[pageid].thumbnail.source) {
        sourceString = data.query.pages[pageid].thumbnail.source;
      } else {
        //use local placeholder thumbnail
        sourceString = '/shield.rgb.black.jpg';
      }
      console.log(sourceString)
      return sourceString;
    } catch (e) {
      if (e instanceof Error) {
        error = e.message;
      } else {
        error = 'An unknown error occurred.';
      }
      return '';
    } finally {
      loading = false;
    }
  }

</script>

<h1>Search Wikipedia</h1>
<div class="flex items-center space-x-2 max-w-xs py-1">
  <Input placeholder="Search Wikipedia..." onkeydown={searchWiki}/>
</div>
<!-- rationalize row height -->
{#each pages as page}
<div class="flex items-center space-x-2 py-1 min-h-24">
  {#await wikiImage(page.pageid.toString())}
    <div class="w-[100px] h-[100px]"></div>
  {:then imageUrl}
    <img src="{imageUrl}" alt="{page.title} wiki thumbnail"/>
  {/await}
  <a href="https://www.wikipedia.org/{page.pageid}" target="_blank">{page.title}</a><ExternalLink />
</div>
{/each}
