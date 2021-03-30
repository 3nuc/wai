<template>
  <span class="dropdown-wrapper">
    <button 
      role="combobox"
      :aria-expanded="isExpanded"
      @click="toggleExpanded" 
      class="dropdown"
      @keydown.up="keydownUp()"
      @keydown.down="keydownDown()"
      @keydown.enter.prevent="keydownEnter()"
      @keydown.esc="isExpanded = false;"
      @blur="onBlur()"
    >
      <slot name="button">
        {{ selectedItem.content }}
      </slot>
    </button>
    <ul v-if="isExpanded" class="list-item-wrapper" :aria-activedescendant="itemUnderCursor?.label ?? undefined">
      <li 
        v-for="(item, index) in items"
        :key="item.label"
        :id="item.label"
        @mouseover="itemSelectionCursor = index"
        @click="onItemClicked" 
        :class="['list-item', {'under-cursor': itemSelectionCursor === index} ]"
        v-text="item.content" 
        role="option" 
      />
    </ul>
  </span>
</template>
<script setup lang="ts">
import { computed, ref, defineProps, defineEmit  } from 'vue'
import type { PropType } from 'vue'
type Item = {
  value: string | number;
  content: string;
}
const props = defineProps({
  items: {
    type: Array as PropType<{value: string | number, content: string }[]>,
    required: true,
  },
  modelValue: [String, Number]
});
const emit = defineEmit(['update:modelValue'])

const selectedItem = computed(() => props.items.find(item => item.value === props.modelValue));
const onItemClicked = (itemIndex: number) => emit('update:modelValue', props.items[itemIndex].value);

const itemSelectionCursor = ref(0);
const itemUnderCursor = computed(() => props.items.find(({value}) => value === itemSelectionCursor.value));

const keydownUp = () => {
  if(itemSelectionCursor.value !== 0) {
    itemSelectionCursor.value--;
  }
}
const keydownDown = () => {
  if(itemSelectionCursor.value + 1 !== props.items.length) {
    itemSelectionCursor.value++;
  }
}
const keydownEnter = () => {
  if(isExpanded.value) {
    onItemClicked(itemSelectionCursor.value);
    isExpanded.value = false;
  }
}

const itemKeydownEnter = (itemIndex: number) => {
  itemSelectionCursor.value = itemIndex;
  isExpanded.value = false;
  emit('update:modelValue', props.items[itemSelectionCursor.value].value);
}

const isExpanded = ref(false);
const toggleExpanded = () => { isExpanded.value = !isExpanded.value };
const onBlur = () => { 
  isExpanded.value = false;
  emit('update:modelValue', props.items[itemSelectionCursor.value].value);
}

</script>
<style scoped>
.dropdown-wrapper {
  width: 150px;
  display: flex;
  flex-direction: column;
}
.dropdown {
}

.list-item-wrapper {
  padding: 0;
  margin: 0;
  display: inline-block;
}

.list-item {
  list-style: none;
  user-select: none;
  cursor: default;
}

.under-cursor {
  background: red;
}
</style>
