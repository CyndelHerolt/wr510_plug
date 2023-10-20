<script setup>
import {onMounted, ref, toRaw} from 'vue'
import axios from 'axios'

let dataSettings = ref('')
let dataStatus = ref('')

onMounted(async () => {
  const responseStatus = await axios.post(
      'https://shelly-86-eu.shelly.cloud/device/status',
      {
        "id": "80646F827174",
        "auth_key": "MWRmYzM2dWlkE62C6C4C76F817CE0A3D2902F5B5D4C115E49B28CF8539114D9246505DE5D368D560D06020A92480"
      },
      {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        }
      }
  );
  dataStatus.value = await responseStatus.data.data.device_status
});


const reboot = async () => {
  try {
    await axios.post(
        'https://shelly-86-eu.shelly.cloud/device/reboot',
        {
          "id": "80646F827174",
          "auth_key": "MWRmYzM2dWlkE62C6C4C76F817CE0A3D2902F5B5D4C115E49B28CF8539114D9246505DE5D368D560D06020A92480"
        },
        {
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
          }
        }
    );
    // You can add a success alert or a console message here
    alert("Successfully rebooted")
  } catch (error) {
    // You can add an error alert or a console message here
    console.error(error)
  }
}
</script>

<template>

  <h2>Status</h2>
  <h3>Temperature</h3>
  <ul>
    <li>{{ dataStatus.temperature }} °</li>
  </ul>
  <h3>Uptime</h3>
  <ul v-if="dataStatus.uptime">
    <li v-if="dataStatus.uptime">
      {{ dataStatus.uptime }} seconds
    </li>
    <li v-else>
      Offline
    </li>
  </ul>
  <h3>WiFi</h3>
  <ul v-if="dataStatus.wifi_sta">
    <li v-if="dataStatus.wifi_sta.connected">
      Online
    </li>
    <li v-else>
      Offline
    </li>
  </ul>
  <h3>Cloud</h3>
  <ul v-if="dataStatus.cloud">
    <li v-if="dataStatus.cloud.enabled">
      Enabled
    </li>
    <li v-else>
      Disabled
    </li>
    <li v-if="dataStatus.cloud.connected">
      Connected
    </li>
    <li v-else>
      Disconnected
    </li>
  </ul>
  <button @click="reboot">Reboot</button>
</template>

<style scoped>

</style>