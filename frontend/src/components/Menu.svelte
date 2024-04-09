<script>
  import keycodes from "./menus/keycodes.svelte";
  export let keyboardKeymap;

  let menus = [
    { name: "Keymaps", component: keycodes },
    { name: "Layouts", component: null },
    { name: "Macros", component: null },
    { name: "Save/Load", component: null },
  ];
  let selectedMenu = menus[0].name;
</script>

<div class="flex flex-row h-full overflow-hidden">
  <ul class="menu grid grid-flow-row auto-rows-min gap-1 h-full bg-base-300 min-w-[7rem] justify-center overflow-y-auto overflow-x-hidden">
    {#each menus as menu}
      <li>
        <!-- svelte-ignore a11y-invalid-attribute -->
        <a href="#" on:click={() => (selectedMenu = menu.name)}>
          {menu.name}
        </a>
      </li>
    {/each}
  </ul>
  {#each menus as menu}
    {#if selectedMenu === menu.name && menu.component}
      <svelte:component this={menu.component} {keyboardKeymap} />
    {/if}
  {/each}
</div>
