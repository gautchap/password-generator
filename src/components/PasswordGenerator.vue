<script setup lang="ts">
import { ref } from 'vue'
import { useToast } from 'vue-toast-notification'

const $toast = useToast()

const password = ref('')

const passwordLength = ref(
  window.localStorage.getItem('passwordLength')
    ? isNaN(Number(window.localStorage.getItem('passwordLength')))
      ? 16
      : Number(window.localStorage.getItem('passwordLength'))
    : 16
)

const savePasswordLength = (event: Event) => {
  let value = Number((event.target as HTMLInputElement).value)
  if (value < 7) return
  passwordLength.value = value
  window.localStorage.setItem('passwordLength', value.toString())
}

const CopyToClipboard = (text: string) => {
  if (!text.trim()) return
  navigator.clipboard.writeText(text)
  $toast.clear()
  $toast.success('Password copied to clipboard', { position: 'top' })
}

const GeneratePassword = () => {
  password.value = ''
  let temp = ''
  const regex = /(?=(.*\d){2})[!@#$%^&*(),.?":{}|<>\s](?=[A-Z]{2,})(?=(.*[a-z]){2,})/g
  const chars = '0123456789abcdefghijklmnopqrstuvwxyz!@#$%^&*(),.?":{}|<>ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  do {
    temp = ''
    window.crypto.getRandomValues(new Uint32Array(passwordLength.value)).forEach((number) => {
      temp += chars[number % chars.length]
    })
  } while (temp.match(regex) === null)

  password.value = temp
}
</script>

<template>
  <main>
    <h1>Random Generator</h1>
    <div>
      <div class="options">
        <input type="checkbox" id="apple" />
        <label for="apple">Apple style</label>
      </div>
      <div class="options">
        <input type="checkbox" id="symbols" />
        <label for="symbols">Symbols (#{[|-_°`\^@]}@!?*²)</label>
      </div>
      <div class="options">
        <input type="checkbox" id="accent" />
        <label for="accent">Accent (éèàîïù)</label>
      </div>
      <div class="options">
        <input
          :value="passwordLength"
          @input="savePasswordLength"
          type="range"
          min="8"
          max="32"
          id="passwordLength"
        />
        <label for="passwordLength">Length : {{ passwordLength }}</label>
      </div>
    </div>
    <input class="mainPassword" v-model="password" type="text" placeholder="Password" />
    <div class="buttonGroup">
      <button @click="GeneratePassword">Generate</button>
      <button @click="CopyToClipboard(password)">Copy</button>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: 2em;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #f2f2f2;
  color: black;
  min-width: 400px;
}

.options {
  display: flex;
  align-items: center;
  gap: 0.5em;
}

.mainPassword {
  height: 2em;
  font-size: 16px;
  width: 100%;
}

@media (max-width: 375px) {
  main {
    min-width: 300px;
  }
}

.buttonGroup {
  margin-top: 1em;
  display: flex;
  gap: 1em;
}

button {
  background-color: hsla(160, 100%, 37%, 1);
  color: white;
  padding: 1em 2em;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  transition: all 0.15s ease-in-out;
  &:hover {
    background-color: hsla(160, 100%, 27%, 1);
  }
}
</style>
