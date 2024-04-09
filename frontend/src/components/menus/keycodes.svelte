<script>
  export let keyboardKeymap;
  import { GetKeyDisplayMap } from "../../lib/wailsjs/go/main/Keyboards";

  let categories = [];
  let selectedCategory = "";
  console.log(keyboardKeymap);

  let customKeycodes;

  let keyDisplayMap = { KeyOrder: [], Categories: [], Keys: [] };
  function getKeyDisplayMap() {
    GetKeyDisplayMap().then((response) => {
      if (response) {
        keyDisplayMap = response;
        console.log(keyDisplayMap);
        categories = keyDisplayMap.Categories;
        selectedCategory = categories[0];
      }
    });
  }

  $: keycodesList = keyDisplayMap.KeyOrder[selectedCategory];
  $: allKeys = keyDisplayMap.Keys;

  /**
   * @param {string} keycode
   */
  function selectKeycode(keycode) {
    console.log(keycode);
  }

  // run on startup
  getKeyDisplayMap();
</script>

<div class="w-full h-full">
  <div class="flex flex-row h-full w-full">
    {#if categories.length !== 0}
      <ul
        class="menu grid grid-flow-row auto-rows-min gap-1 h-full bg-neutral text-neutral-content min-w-[7rem] justify-center overflow-y-auto overflow-x-hidden"
      >
        {#each categories as cat}
          {#if cat !== "Custom" || (cat === "Custom" && customKeycodes)}
            <li class="custom-flex-item scroll-px-1.5">
              <!-- svelte-ignore a11y-invalid-attribute -->
              <a href="#" on:click={() => (selectedCategory = cat)}>
                {cat}
              </a>
            </li>
          {/if}
        {/each}
      </ul>
      {#each categories as cat}
        {#if selectedCategory === cat}
          <!-- {cat} -->
          {#if keyDisplayMap}
            <div
              class="grid grid-flow-row auto-rows-min gap-2 w-full custom-grid min-w-max justify-center p-3 overflow-y-auto overflow-x-hidden"
            >
              {#each keycodesList as key}
                {#if allKeys[key] !== undefined}
                  <button
                    class="border-2 w-16 h-16 text-sm rounded-lg select-none"
                    on:click={() => selectKeycode(key)}
                  >
                    {#if allKeys[key]?.Center !== ""}
                      {allKeys[key].Center}
                    {:else if allKeys[key]?.TopMiddle !== ""}
                      <div class="grid grid-rows-2 aspect-square p-2 justify-evenly content-evenly">
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].TopMiddle}
                        </div>
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].BottomMiddle}
                        </div>
                      </div>
                    {:else}
                      <div class="grid grid-rows-2 grid-cols-2 aspect-square p-2 justify-evenly content-evenly">
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].TopLeft}
                        </div>
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].TopRight}
                        </div>
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].BottomLeft}
                        </div>
                        <div class="flex justify-center items-center text-center">
                          {allKeys[key].BottomRight}
                        </div>
                      </div>
                    {/if}
                  </button>
                {/if}
              {/each}
            </div>
          {/if}
        {/if}
      {/each}
    {/if}
  </div>
</div>

<style>
  .custom-grid {
    grid-template-columns: repeat(auto-fill, 64px);
  }

  .custom-flex-item {
    flex-grow: 0; /* Equivalent to flex-grow-0 */
    flex-shrink: 0; /* Equivalent to flex-shrink-0 */
    flex-basis: auto; /* Equivalent to flex-auto */
    width: 100%;
  }
</style>
