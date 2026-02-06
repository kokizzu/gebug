<script setup lang="ts">
import { ref, onMounted } from 'vue'
import ConfigService, { type Config } from './services/ConfigService'

const config = ref<Config>({
  name: '',
  outputBinary: '/app',
  buildCommand: 'go build -o /app',
  runCommand: '/app',
  runtimeImage: 'golang:1.23',
  debuggerEnabled: false,
  debuggerPort: 40000,
  networks: [],
  exposePorts: [],
  environment: []
})

const location = ref('')
const message = ref('')
const messageType = ref<'success' | 'error'>('success')

onMounted(async () => {
  try {
    const data = await ConfigService.get()
    if (data.config) {
      config.value = data.config
      location.value = data.location || ''
    }
  } catch (err) {
    console.error('Failed to load config:', err)
  }
})

const addPort = () => {
  config.value.exposePorts.push({ port: 8080 })
}

const removePort = (index: number) => {
  config.value.exposePorts.splice(index, 1)
}

const addNetwork = () => {
  config.value.networks.push({ network: '' })
}

const removeNetwork = (index: number) => {
  config.value.networks.splice(index, 1)
}

const addEnvVar = () => {
  config.value.environment.push({ envName: '', envValue: '' })
}

const removeEnvVar = (index: number) => {
  config.value.environment.splice(index, 1)
}

const saveConfig = async () => {
  try {
    await ConfigService.save(config.value)
    message.value = 'Configuration saved successfully!'
    messageType.value = 'success'
    setTimeout(() => {
      message.value = ''
    }, 3000)
  } catch (err) {
    message.value = 'Failed to save configuration'
    messageType.value = 'error'
    setTimeout(() => {
      message.value = ''
    }, 3000)
  }
}
</script>

<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <h1 class="mb-4">Gebug Configuration</h1>

        <div v-if="location" class="alert alert-info mb-4">
          <strong>Project Location:</strong> {{ location }}
        </div>

        <div v-if="message" :class="`alert alert-${messageType === 'success' ? 'success' : 'danger'}`">
          {{ message }}
        </div>

        <form @submit.prevent="saveConfig" class="needs-validation" novalidate>
          <div class="mb-3">
            <label for="name" class="form-label">Name</label>
            <input
              type="text"
              class="form-control"
              id="name"
              v-model="config.name"
              required
            >
          </div>

          <div class="mb-3">
            <label for="runtimeImage" class="form-label">Runtime Image</label>
            <input
              type="text"
              class="form-control"
              id="runtimeImage"
              v-model="config.runtimeImage"
              placeholder="golang:1.23"
              required
            >
          </div>

          <div class="mb-3">
            <label for="outputBinary" class="form-label">Output Binary</label>
            <input
              type="text"
              class="form-control"
              id="outputBinary"
              v-model="config.outputBinary"
              required
            >
          </div>

          <div class="mb-3">
            <label for="buildCommand" class="form-label">Build Command</label>
            <input
              type="text"
              class="form-control"
              id="buildCommand"
              v-model="config.buildCommand"
              required
            >
          </div>

          <div class="mb-3">
            <label for="runCommand" class="form-label">Run Command</label>
            <input
              type="text"
              class="form-control"
              id="runCommand"
              v-model="config.runCommand"
              required
            >
          </div>

          <div class="mb-3 form-check">
            <input
              type="checkbox"
              class="form-check-input"
              id="debuggerEnabled"
              v-model="config.debuggerEnabled"
            >
            <label class="form-check-label" for="debuggerEnabled">
              Enable Debugger
            </label>
          </div>

          <div v-if="config.debuggerEnabled" class="mb-3">
            <label for="debuggerPort" class="form-label">Debugger Port</label>
            <input
              type="number"
              class="form-control"
              id="debuggerPort"
              v-model.number="config.debuggerPort"
            >
          </div>

          <div class="mb-3">
            <label class="form-label">Expose Ports</label>
            <div v-for="(port, index) in config.exposePorts" :key="index" class="input-group mb-2">
              <input
                type="number"
                class="form-control"
                v-model.number="port.port"
                placeholder="8080"
              >
              <button
                type="button"
                class="btn btn-outline-danger"
                @click="removePort(index)"
              >
                Remove
              </button>
            </div>
            <button type="button" class="btn btn-outline-primary btn-sm" @click="addPort">
              Add Port
            </button>
          </div>

          <div class="mb-3">
            <label class="form-label">Networks</label>
            <div v-for="(network, index) in config.networks" :key="index" class="input-group mb-2">
              <input
                type="text"
                class="form-control"
                v-model="network.network"
                placeholder="network-name"
              >
              <button
                type="button"
                class="btn btn-outline-danger"
                @click="removeNetwork(index)"
              >
                Remove
              </button>
            </div>
            <button type="button" class="btn btn-outline-primary btn-sm" @click="addNetwork">
              Add Network
            </button>
          </div>

          <div class="mb-3">
            <label class="form-label">Environment Variables</label>
            <div v-for="(env, index) in config.environment" :key="index" class="input-group mb-2">
              <input
                type="text"
                class="form-control"
                v-model="env.envName"
                placeholder="KEY"
              >
              <input
                type="text"
                class="form-control"
                v-model="env.envValue"
                placeholder="value"
              >
              <button
                type="button"
                class="btn btn-outline-danger"
                @click="removeEnvVar(index)"
              >
                Remove
              </button>
            </div>
            <button type="button" class="btn btn-outline-primary btn-sm" @click="addEnvVar">
              Add Variable
            </button>
          </div>

          <div class="d-grid">
            <button type="submit" class="btn btn-primary btn-lg">
              Save Configuration
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  color: #333;
  font-weight: 600;
}

.form-label {
  font-weight: 500;
  color: #495057;
}

.btn-primary {
  background-color: #0d6efd;
  border-color: #0d6efd;
}

.btn-primary:hover {
  background-color: #0b5ed7;
  border-color: #0a58ca;
}
</style>
