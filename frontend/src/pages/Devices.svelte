<script>
  import {
    GetHidDevices,
    GetActiveDevices,
    RefreshAllDevices,
  } from "../lib/wailsjs/go/main/Keyboards";

  let activeDevices = [];
  let allDevices = [];
  // on page load, get all HID devices
  function getDevices() {
    GetHidDevices().then((response) => {
      allDevices = response;
    });
    GetActiveDevices().then((response) => {
      activeDevices = response;
      console.log(activeDevices);
    });
  }

  function refreshDevices() {
    RefreshAllDevices();
    setTimeout(getDevices, 1000);
  }

  getDevices();
</script>

<main>
  <div role="tablist" class="tabs tabs-bordered">
    <input
      type="radio"
      name="devices_tabs"
      role="tab"
      class="tab"
      aria-label="Found"
      checked
    />
    <div role="tabpanel" class="tab-content p-10">
      {#if activeDevices && activeDevices.length > 0}
        <div class="overflow-x-auto">
          <table class="table">
            <thead>
              <tr>
                <th>VID</th>
                <th>PID</th>
                <th>MFR</th>
                <th>Product</th>
                <th>Protocol</th>
                <th>Version</th>
                <th>KeycodeVer</th>
                <th>Support</th>
              </tr>
            </thead>
            <tbody>
              {#each activeDevices as device}
                <tr>
                  <td
                    >{`0x${device.deviceInfo.vendorID.toString(16).padStart(4, "0").toUpperCase()}`}</td
                  >
                  <td
                    >{`0x${device.deviceInfo.productID.toString(16).padStart(4, "0").toUpperCase()}`}</td
                  >
                  <td>{device.deviceInfo.mfrStr}</td>
                  <td>{device.deviceInfo.productStr}</td>
                  <td>{device.protocol}</td>
                  <td>{device.supported.version}</td>
                  <td>{device.supported.keycodeVersion}</td>
                  <td>
                    {#if device.supported.supportEnum == 0}
                      ❌ Missing
                    {:else if device.supported.supportEnum == 1}
                      🔗 Sideloaded
                    {:else if device.supported.supportEnum == 2}
                      ✅ Official
                    {/if}
                  </td>
                </tr>
              {/each}
            </tbody>
          </table>
        </div>
      {:else}
        <p>No devices found</p>
      {/if}
    </div>

    <input
      type="radio"
      name="devices_tabs"
      role="tab"
      class="tab"
      aria-label="All"
    />
    <div role="tabpanel" class="tab-content p-10">
      {#if allDevices && allDevices.length > 0}
        <div class="overflow-x-auto">
          <table class="table">
            <thead>
              <tr>
                <th>VID</th>
                <th>PID</th>
                <th>MFR</th>
                <th>Product</th>
                <th>Release</th>
                <th>Usage</th>
                <th>UsagePage</th>
                <th>IntNum</th>
              </tr>
            </thead>
            <tbody>
              {#each allDevices as device}
                {#if device.mfrStr && device.productStr}
                  <tr>
                    <td
                      >{`0x${device.vendorID.toString(16).padStart(4, "0").toUpperCase()}`}</td
                    >
                    <td
                      >{`0x${device.productID.toString(16).padStart(4, "0").toUpperCase()}`}</td
                    >
                    <td>{device.mfrStr}</td>
                    <td>{device.productStr}</td>
                    <td>{device.releaseNbr}</td>
                    <td>{device.usage}</td>
                    <td>{device.usagePage}</td>
                    <td>{device.interfaceNbr}</td>
                  </tr>
                {/if}
              {/each}
            </tbody>
          </table>
        </div>
      {:else}
        <p>No devices found</p>
      {/if}
    </div>

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
</main>
