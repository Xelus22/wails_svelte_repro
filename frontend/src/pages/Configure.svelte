<script>
  import {
    GetActiveDevices,
    RefreshAllDevices,
    SelectDevice,
    GetSelectedDeviceConfig,
  } from "../lib/wailsjs/go/main/Keyboards";

  import Menu from "../components/Menu.svelte";
  import { PaneGroup, Pane, PaneResizer } from "paneforge";

  let activeDevices = [];
  let selectedValue = null;
  let selectedDevicePath = null;
  let selectedDeviceConfig = null;

  // on page load, get all HID devices
  function getDevices() {
    GetActiveDevices().then((response) => {
      activeDevices = response;

      if (activeDevices && activeDevices.length > 0) {
        selectedDevicePath = activeDevices[0].deviceInfo.path;
        selectedValue =
          activeDevices[0].deviceInfo.mfrStr +
          " " +
          activeDevices[0].deviceInfo.productStr;

        SelectDevice(selectedDevicePath);
      } else {
        selectedDevicePath = "";
        selectedValue = "No devices found";
      }

      console.log(selectedDevicePath);
      console.log(selectedValue);

      if (selectedDevicePath && selectedDevicePath !== "") {
        // only get selected device is there is a selected device
        GetSelectedDeviceConfig().then((response) => {
          if (response) {
            selectedDeviceConfig = response;
            console.log("selectedDeviceConfig", selectedDeviceConfig);
          }
        });
      }
    });
  }

  /**
   * @param {string} selectedDevicePath
   */
  function changeSelectedDevice(selectedDevicePath) {
    SelectDevice(selectedDevicePath);
    GetSelectedDeviceConfig().then((response) => {
      if (response) {
        selectedDeviceConfig = response;
        console.log("selectedDeviceConfig", selectedDeviceConfig);
      }
    });
  }

  function refreshDevices() {
    RefreshAllDevices();
    setTimeout(getDevices, 1000);
  }

  getDevices();
</script>

<div class="min-h-full flex flex-col">
  <div class="flex justify-center">
    <h1>Configure</h1>
    <select
      bind:value={selectedDevicePath}
      on:change={() => changeSelectedDevice(selectedDevicePath)}
      class="select select-bordered w-full max-w-xs"
    >
      {#if activeDevices && activeDevices.length > 0}
        {#each activeDevices as device}
          <option value={device.deviceInfo.path}
            >{device.deviceInfo.mfrStr} {device.deviceInfo.productStr}</option
          >
        {/each}
      {:else}
        <option disabled selected value="">No devices found</option>
      {/if}
    </select>
    <button
      on:click={refreshDevices}
      class="tooltip tooltip-right h-max"
      data-tip="Refresh"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="23"
        height="23"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path
          d="M21.5 2v6h-6M2.5 22v-6h6M2 11.5a10 10 0 0 1 18.8-4.3M22 12.5a10 10 0 0 1-18.8 4.2"
        />
      </svg>
    </button>
  </div>
  <PaneGroup
    direction="vertical"
    autoSaveId="configure"
    class="w-full flex-grow box-border"
  >
    <Pane defaultSize={50} class="bg-base-100 box-border">
      <div class="flex h-full items-center justify-center">
        <span class="font-semibold">Top</span>
      </div>
    </Pane>
    <PaneResizer
      class="relative flex h-2 items-center justify-center bg-background"
    >
      <div
        class="z-10 flex h-5 w-7 items-center justify-center rounded-sm border bg-brand"
      >
        ...
      </div>
    </PaneResizer>
    <Pane defaultSize={50} class="bg-base-100 box-border">
      <div class="flex h-full">
        <span class="font-semibold h-full w-full">
          {#if selectedDeviceConfig}
            <Menu keyboardKeymap={selectedDeviceConfig.keycodes} />
          {/if}
        </span>
      </div>
    </Pane>
  </PaneGroup>
</div>
