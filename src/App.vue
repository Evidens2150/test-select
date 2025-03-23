<template>
  <div class="container">
    <div class="form">
      <CustomSelect
        v-model="statusId"
        label="Статус поставки"
        close-item-text="Статус"
        :options="statusList"
        @update:model-value="handleUpdateStatus"
      />
      <CustomSelect
        v-model="movementId"
        label="Режим перемещения"
        close-item-text="Режим"
        :options="movementList"
        @update:model-value="handleUpdateMovement"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
// Modules
import { ref, computed, toRefs, defineProps, withDefaults, getCurrentInstance } from 'vue';
// Components
import CustomSelect from '@/components/CustomSelect.vue';
// Types
import { ISelectOption } from "@/types/select";

// Data
const statusList: ISelectOption[] = [
  {
    id: 0,
    description: "Формируется"
  },
  {
    id: 10,
    description: "Товар прибыл"
  },
  {
    id: 20,
    description: "Товар прибыл (срочная поставка)"
  },
  {
    id: 30,
    description: "Оформление ДТ"
  },
  {
    id: 40,
    description: "ДТ выпущена"
  },
  {
    id: 50,
    description: "Поставка закрыта"
  }
];

const movementList: ISelectOption[] = [
  {
    id: 1,
    description: "Импорт"
  },
  {
    id: 2,
    description: "Экспорт"
  },
];

const statusId = ref<number | null>(null);

const movementId = ref<number | null>(null);

const status = ref<ISelectOption | null>(null);

const movement = ref<ISelectOption | null>(null);

// Methods
const handleUpdateStatus = (statusId: number) => {
  status.value = statusList.find(option => option.id === statusId) || null;
};

const handleUpdateMovement = (movementId: number) => {
  movement.value = movementList.find(option => option.id === movementId) || null;

};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
}

body * {
  box-sizing: border-box;
  transition: all .15s linear;
}

.container {
  width: 100%;
  min-height: 100vh;
  padding: 40px 20px;
}

.form {
  width: 100%;

  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
  gap: 40px;
}
</style>
