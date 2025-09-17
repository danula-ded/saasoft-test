<script setup lang="ts">
import { ref, computed } from 'vue'
import IconDelete from './icons/IconDelete.vue'
import { useAccountStore } from '@/stores/accountStore'
import { storeToRefs } from 'pinia'
const accountStore = useAccountStore()
const deleteData = (id: number) => accountStore.deleteData(id)
const setNullPass = (type: string, id: number) => accountStore.setNullPass(type, id)
const { options } = storeToRefs(accountStore)

const selectTarget = ref('')
const obj = defineModel({ type: Object })

const tags = computed({
  get() {
    if (Array.isArray(obj.value.tags)) return obj.value.tags.join(';')
    return null
  },
  set(value) {
    obj.value.tags = value.length > 0 ? value.split(';') : []
  },
})

const handleBlur = (event: Event) => {
  const input = event.target as HTMLInputElement
  if (!input.value.trim()) {
    input.classList.add('error-color')
  } else {
    input.classList.remove('error-color')
  }
}

const hadlerSelect = (event: Event) => {
  const target = event.target as HTMLElement
  if (target.nodeName === 'SELECT') {
    selectTarget.value = target
    selectTarget.value.classList.remove('error-color')
  } else {
    if (event.target.value === '') {
      selectTarget.value.classList.add('error-color')
    } else {
      selectTarget.value.classList.remove('error-color')
    }
  }
}
</script>
<template>
  <div class="grid grid-cols-12 gap-4 items-center">
    <!-- Метки -->
    <div class="col-span-3">
      <input 
        v-model="tags" 
        @blur="handleBlur" 
        maxlength="50" 
        placeholder="Введите метки"
        class="input-focus w-full h-10 bg-white/20 border border-white/30 rounded-lg px-3 text-white placeholder-white/60 focus:bg-white/30" 
      />
    </div>
    
    <!-- Тип записи -->
    <div class="col-span-3">
      <select 
        @click="hadlerSelect($event)" 
        v-model="obj.type" 
        class="input-focus w-full h-10 bg-white/20 border border-white/30 rounded-lg px-3 text-white cursor-pointer focus:bg-white/30"
      >
        <option value="" class="bg-gray-800 text-white">Выберите опцию</option>
        <option 
          @click="setNullPass(obj.type, obj.id)" 
          v-for="option in options" 
          :key="option.text" 
          :value="option.text"
          class="bg-gray-800 text-white"
        >
          {{ option.text }}
        </option>
      </select>
    </div>
    
    <!-- Логин -->
    <div class="col-span-3">
      <input 
        v-model="obj.login" 
        @blur="handleBlur" 
        maxlength="100" 
        placeholder="Введите логин"
        class="input-focus w-full h-10 bg-white/20 border border-white/30 rounded-lg px-3 text-white placeholder-white/60 focus:bg-white/30" 
      />
    </div>
    
    <!-- Пароль -->
    <div class="col-span-3">
      <input 
        v-show="obj.type != 'Local'" 
        @blur="handleBlur" 
        v-model="obj.pass" 
        type="password" 
        maxlength="50" 
        placeholder="Введите пароль"
        class="input-focus w-full h-10 bg-white/20 border border-white/30 rounded-lg px-3 text-white placeholder-white/60 focus:bg-white/30" 
      />
    </div>
    
    <!-- Кнопка удаления -->
    <div class="col-span-12 sm:col-span-0 flex justify-center sm:justify-end mt-2 sm:mt-0">
      <button 
        @click="deleteData(obj.id)" 
        class="button-hover bg-red-500/20 hover:bg-red-500/40 border border-red-400/30 hover:border-red-400/50 text-red-300 hover:text-red-200 p-2 rounded-lg transition-all duration-300"
      >
        <IconDelete class="w-4 h-4" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.error-color {
  border-color: #ef4444 !important;
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.1) !important;
  background-color: rgba(239, 68, 68, 0.1) !important;
}

/* Кастомные стили для select */
select option {
  background-color: #1f2937;
  color: white;
}

/* Стили для placeholder */
input::placeholder {
  color: rgba(255, 255, 255, 0.6);
}

/* Анимация появления строки */
.row-enter-active {
  transition: all 0.3s ease;
}

.row-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
