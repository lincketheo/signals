<script lang="ts" setup>
import { reactive, toRaw } from 'vue' // ← add toRaw
import TypeEditor from '@/components/TypeEditor.vue'
import { type Type } from '@/lib/types'

interface CreateForm {
  title: string
  description: string
  root: Type
}

const form = reactive<CreateForm>({
  title: '',
  description: '',
  root: { kind: 'scalar', name: 'U32' }, // initial placeholder
})

function deepClone<T>(v: T): T {
  // JSON trick: fine for a schema tree and strips proxies
  return JSON.parse(JSON.stringify(toRaw(v)))
}

function submit() {
  const payload = deepClone(form) // ← now it’s plain data
  console.log('SUBMIT', payload)

  // TODO send `payload` to your API
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center bg-gray-100 p-8">
    <h1 class="text-3xl font-semibold mb-8">Create Dataset</h1>

    <form
      class="w-full max-w-2xl bg-white rounded-2xl shadow p-8 space-y-6"
      @submit.prevent="submit"
    >
      <input v-model="form.title" class="w-full border rounded px-3 py-2" placeholder="Title" />

      <textarea
        v-model="form.description"
        class="w-full border rounded px-3 py-2"
        placeholder="Description"
        rows="3"
      />

      <!-- 🔄 full recursive editor for the dataset root -->
      <TypeEditor v-model="form.root" />

      <button class="w-full bg-green-600 text-white py-3 rounded hover:bg-green-700" type="submit">
        Save
      </button>
    </form>
  </div>
</template>
