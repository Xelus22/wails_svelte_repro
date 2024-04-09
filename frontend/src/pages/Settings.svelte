<script>
  const appThemes = [
    "default",
    "light",
    "dark",
    "cupcake",
    "bumblebee",
    "emerald",
    "corporate",
    "synthwave",
    "retro",
    "cyberpunk",
    "valentine",
    "halloween",
    "garden",
    "forest",
    "aqua",
    "lofi",
    "pastel",
    "fantasy",
    "wireframe",
    "black",
    "luxury",
    "dracula",
    "cmyk",
    "autumn",
    "business",
    "acid",
    "lemonade",
    "night",
    "coffee",
    "winter",
    "dim",
    "nord",
    "sunset",
  ];
  let appTheme = appThemes[0];

  let selectedKBTheme = "Default";

  const KBThemes = ["Default", "Olivia"];

  function changeSelectedAppTheme(theme) {
    appTheme = theme;
    document.documentElement.setAttribute("data-theme", theme.toLowerCase());
  }

  function changeSelectedKBTheme(theme) {
    selectedKBTheme = theme;
  }
</script>

<div>
  <h1>Settings</h1>
  <h2>App Theme</h2>
  <select
    bind:value={appTheme}
    on:change={() => changeSelectedAppTheme(appTheme)}
    class="select select-bordered w-full max-w-xs"
  >
    {#each appThemes as theme}
      <option class="theme-controller" value={theme}>{theme}</option>
    {/each}
  </select>
  <br />
  <h2>Keyboard Theme</h2>
  <select
    bind:value={selectedKBTheme}
    on:change={() => changeSelectedKBTheme(selectedKBTheme)}
    class="select select-bordered w-full max-w-xs"
  >
    {#each KBThemes as theme}
      <option value={theme}>{theme}</option>
    {/each}
  </select>
</div>
