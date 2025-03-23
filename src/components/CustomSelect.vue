<template>
  <div class="custom-select">
    <label
      v-if="label"
      :for="computedId"
      class="custom-select__label"
    >
      {{ label }}
    </label>
    <p 
      :id="computedId"
      class="custom-select__trigger"
      tabindex="0"
      @focus="handleOpenDropdown"
      @blur="handleBlur"
    >
      {{ selectedOptionDescription }}
    </p>
    <div
      class="custom-select__dropdown"
      :class="{'custom-select__dropdown--active': showDropdown}"
    >
      <div class="custom-select__close-item">
        <span>
          {{ closeItemText }}
        </span>
        <button
          tabindex="-1"
          class="custom-select__close-button"
          @click.stop="handleCloseDropdown"
        >
          ✖
        </button>
      </div>
      <ul
          tabindex="-1"
        class="custom-select__option-list"
      >
        <li
          v-for="option in filteredOptions"
          :key="option.id"
          @click="handleSelectOption(option)"
          class="custom-select__option"
        >
          {{ option.description }}
        </li>
      </ul>
    </div>
    <div
      v-if="showDropdown"
      class="custom-select__overlay"
      @click.stop="handleCloseDropdown"
    >
    </div>
  </div>
</template>

<script setup lang="ts">
// Modules
import { ref, computed, toRefs, defineProps, withDefaults, getCurrentInstance } from 'vue';
// Types
import { ISelectOption } from '@/types/select';

// Props
const props = withDefaults(defineProps<{
  label: string;
  closeItemText: string;
  options: ISelectOption[];
}>(), {
  label: '',
  closeItemText: '',
  options: () => ([]),
});

const {
  label,
  closeItemText,
  options,
} = toRefs(props);

// Emits
const emits = defineEmits<{
  (e: 'update:modelValue', value: number): void;
}>();

// Model
const modelValue = defineModel<number | null>();

// Data
const showDropdown = ref(false);
const filteredOptions = ref(options.value);

// Computed
const computedId = computed((): string => {
  const componentInstance = getCurrentInstance();
  const uid = componentInstance?.uid || 0;

  return `select-${uid}`
});

const selectedOptionDescription = computed((): string => {
  if (typeof modelValue.value !== 'number') return '';

  return options.value.find(option => option.id === modelValue.value)?.description || '';
});

// Methods
const handleOpenDropdown = () => {
  showDropdown.value = true;
};

const handleCloseDropdown = () => {
  showDropdown.value = false;
};

const handleSelectOption = (option: ISelectOption) => {
  // showDropdown.value = false;
  emits('update:modelValue', option.id)
};

const handleBlur = () => {
  setTimeout(() => {
    handleCloseDropdown();
  }, 100);
};
</script>

<style lang='scss'>
.custom-select {
  position: relative;
  width: 300px;
}

.custom-select__label {
  position: absolute;
  left: 4px;
  top: 4px;

  font-size: 11px;
  font-weight: 700;
}

.custom-select__trigger {
  min-height: 40px;
  width: 100%;
  padding: 16px 4px 4px;
  margin: 0;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: white;
  cursor: pointer;
  text-align: left;
}

.custom-select__overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;

  background-color: transparent;
}

.custom-select__dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;

  width: 100%;
  max-height: 0;
  margin-top: 3px;

  z-index: 2;
  overflow: hidden;
}

.custom-select__dropdown--active {
  max-height: 240px;
  box-shadow: 0px 8px 16px 4px rgba(21, 45, 91, 0.16);
}

.custom-select__option-list {
  width: 100%;
  max-height: 200px;
  padding: 0;
  margin: 0;

  border: 1px solid #ccc;
  background: white;
  list-style-type: none;

  overflow-y: auto;
}

.custom-select__option {
  padding: 8px;
  cursor: pointer;
  text-align: start;

  &:not(:first-child) {
    border-top: 1px solid #ccc;
  }

  &:hover {
    background-color: #f0f0f0;
  }
}

.custom-select__close-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 8px;

  border: 1px solid #ccc;
  border-bottom: none;

  font-weight: 600;
}

.custom-select__close-button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  padding: 0;

  opacity: .5;

  &:hover {
    opacity: 1;
  }
}
</style>