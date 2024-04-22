<script setup lang="ts">
import { ref } from 'vue'

const password = ref('')

const passwordLength = ref(16)

const CopyToClipboard = (text: string) => {
  if (!text.trim()) return
  navigator.clipboard.writeText(text)
}

const GeneratePassword = () => {
  password.value = ''
  const chars = '0123456789abcdefghijklmnopqrstuvwxyz!@#$%^&*()ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  window.crypto.getRandomValues(new Uint32Array(passwordLength.value)).forEach((number) => {
    password.value += chars[number % chars.length]
  })
}
</script>

<template>
  <main>
    <h1>hello password</h1>
    <div>
      <div>
        <input type="checkbox" id="apple" />
        <label for="apple">Apple style</label>
      </div>
      <div>
        <input type="checkbox" id="symbols" />
        <label for="symbols">Symbols (#{[|-_°`\^@]}@!?*²)</label>
      </div>
      <div>
        <input type="checkbox" id="accent" />
        <label for="accent">Accent (éèàîïù)</label>
      </div>
      <div>
        <input v-model="passwordLength" type="number" id="passwordLength" />
        <label for="passwordLength">Lenght</label>
      </div>
    </div>
    <input v-model="password" type="text" placeholder="Password" />
    <div>
      <button @click="GeneratePassword">Generate</button>
      <button @click="CopyToClipboard(password)">Copy</button>
    </div>
  </main>
</template>

<style scoped>
main {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #f2f2f2;
  color: black;
  height: 200px;
  width: 400px;
}

@media (max-width: 375px) {
  main {
    width: 300px;
  }
}
</style>
