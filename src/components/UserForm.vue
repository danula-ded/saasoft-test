<script setup lang="ts">
import IconPlus from './icons/IconPlus.vue'
import IconSign from './icons/IconSign.vue'
import FormRow from './FormRow.vue'
import { useAccountStore } from '@/stores/accountStore'
import { storeToRefs } from 'pinia'

const accountStore = useAccountStore()
const { data } = storeToRefs(accountStore)
const addData = () => accountStore.addData()
</script>

<template>
  <div class="glass-effect card-shadow rounded-3xl p-6 sm:p-10 text-white fade-in-up">
    <!-- Заголовок -->
    <div class="flex items-center justify-between mb-6">
      <h1 class="text-2xl sm:text-4xl font-bold bg-gradient-to-r from-white to-blue-200 bg-clip-text text-transparent">
        Учетные записи
      </h1>
      <button 
        @click="addData" 
        class="button-hover bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700 text-white p-3 rounded-full shadow-lg"
      >
        <IconPlus class="w-6 h-6" />
      </button>
    </div>

    <!-- Информационное сообщение -->
    <div class="flex items-start gap-3 mb-6 p-4 bg-blue-500/20 rounded-xl border border-blue-400/30">
      <IconSign class="w-5 h-5 text-blue-300 mt-0.5 flex-shrink-0" />
      <p class="text-sm text-blue-100 leading-relaxed">
        Для указания нескольких меток для одной пары логин/пароль используйте разделитель ;
      </p>
    </div>

    <!-- Таблица -->
    <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-4 sm:p-6 border border-white/20">
      <!-- Заголовки колонок -->
      <div class="grid grid-cols-12 gap-4 mb-4 pb-3 border-b border-white/20">
        <div class="col-span-3">
          <div class="text-sm font-semibold text-white/90">Метки</div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-white/90">
            <span class="hidden sm:inline">Тип записи</span>
            <span class="sm:hidden">Тип</span>
          </div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-white/90">Логин</div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-white/90">Пароль</div>
        </div>
      </div>

      <!-- Строки данных -->
      <div class="space-y-3">
        <FormRow v-for="(obj, key) in data" :key="obj.id" v-model="data[key]" />
      </div>
    </div>

    <!-- Отладочная информация -->
    <div v-if="data[0]" class="mt-4 p-3 bg-gray-500/20 rounded-lg">
      <span class="text-xs text-gray-300">Отладка: {{ data[0].tags }}</span>
    </div>
  </div>
</template>

<style scoped>
/* Дополнительные стили для компонента */
.glass-effect {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Анимация появления */
.fade-in-up {
  animation: fadeInUp 0.8s ease-out;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Стили для градиентного текста */
.bg-clip-text {
  -webkit-background-clip: text;
  background-clip: text;
}

/* Hover эффекты для кнопок */
.button-hover {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.button-hover:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

/* Адаптивные стили */
@media (max-width: 640px) {
  .glass-effect {
    margin: 1rem;
    padding: 1.5rem;
  }
}
</style>
