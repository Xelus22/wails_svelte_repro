<script>
    import {
      GetActiveDevices,
      RefreshAllDevices,
    } from "../lib/wailsjs/go/main/Keyboards";
  
    let activeDevices = [];
    // on page load, get all HID devices
    function getDevices() {
      GetActiveDevices().then((response) => {
        activeDevices = response;
      });
    }
  
    function refreshDevices() {
      RefreshAllDevices();
      setTimeout(getDevices, 1000);
    }
  
    getDevices();
  </script>
<main>
    <h1>Test</h1>
    <button on:click={refreshDevices}>refresh</button>
    {#if activeDevices && activeDevices.length > 0}
        {#each activeDevices as device}
            <p>{device.deviceInfo.vendorID} </p>
            <p>{device.deviceInfo.productID} </p>
            <p>{device.deviceInfo.mfrStr} </p>
            <p>{device.deviceInfo.productStr} </p>
            <p>{device.protocol} </p>
            <p>{device.supported.version} </p>
            <p>{device.supported.keycodeVersion} </p>
        {/each}
    {/if}
</main>