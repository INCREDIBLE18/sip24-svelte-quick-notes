<script>
    import { onMount } from 'svelte';
  
    let pages = [];
    let currentPageIndex = 0;
    let title = '';
    let note = '';
  
    onMount(() => {
      const savedPages = localStorage.getItem("pages");
      if (savedPages) {
        pages = JSON.parse(savedPages);
        title = pages[currentPageIndex];
        note = localStorage.getItem(title);
      } else {
        addPage();
      }
    });
  
    function saveNote() {
      const storedPageName = pages[currentPageIndex];
      if (storedPageName != title) {
        localStorage.removeItem(storedPageName);
        pages[currentPageIndex] = title;
      }
      localStorage.setItem(title, note);
      localStorage.setItem("pages", JSON.stringify(pages));
    }
  
    function addPage() {
      pages.push("New Page");
      selectPage(pages.length ? pages.length - 1 : 0);
    }
  
    function selectPage(index) {
      currentPageIndex = index;
      title = pages[currentPageIndex];
      note = localStorage.getItem(title);
    }
  </script>
  
  <aside class="fixed top-0 left-0 z-40 w-60 h-screen">
    <div class="bg-sidebar overflow-y-auto py-5 px-3 h-full border-r border-gray-200 shadow-lg">
      <ul class="space-y-2">
        {#each pages as page, index}
        <li>
          <button on:click={() => selectPage(index)} class="{index == currentPageIndex ? 'bg-selected-page' : 'bg-page'} py-2 px-3 text-white rounded-lg hover:bg-hover-page transition-all duration-300">{page}</button>
        </li>
        {/each}
        <li class="text-center">
          <button on:click={addPage} class="font-medium text-white hover:text-gray-400 transition-all duration-300">+ Add page</button>
        </li>
      </ul>
    </div>
  </aside>
  
  <main class="p-4 ml-60 h-auto">
    <div class="grid grid-cols-2 items-center mb-3">
      <h1 class="text-3xl font-bold text-black bg-white p-2 rounded-lg" contenteditable bind:textContent={title}></h1>
      <button class="ml-auto bg-primary text-white px-5 py-2.5 rounded-lg font-medium text-sm mt-3 hover:bg-primary transition-all duration-300" on:click={saveNote}>Save</button>
    </div>
    <hr class="border-gray-600"/>
    <textarea class="mt-3 block w-full bg-textarea border border-gray-600 rounded-lg text-white p-2.5" bind:value={note}></textarea>
  </main>
  
  <style>
    :root {
      --primary: #1F2937;
      --sidebar: #2D3748;
      --page: #4A5568;
      --selected-page: #1A202C;
      --hover-page: #718096;
      --textarea: #2D3748;
    }
  
    .bg-sidebar {
      background: var(--sidebar);
    }
  
    .bg-page {
      background: var(--page);
    }
  
    .bg-selected-page {
      background: var(--selected-page);
    }
  

  
    .bg-primary {
      background: var(--primary);
    }
  
    .bg-textarea {
      background: var(--textarea);
    }
  </style>
  