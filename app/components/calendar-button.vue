<template>
  <Popover v-model:open="open">
    <PopoverTrigger as-child>
      <Button
        variant="outline"
        role="combobox"
        :aria-expanded="open"
        class="justify-between"
        v-bind="$attrs"
      >
        {{ formattedDate }}
        <ChevronsUpDownIcon class="opacity-50" />
      </Button>
    </PopoverTrigger>
    <PopoverContent class="w-md p-0">
      <Calendar
        class="rounded-lg border"
        locale="uk-UA"
        :default-placeholder="placeholder"
        @update:model-value="setSelectedDate"
      />
    </PopoverContent>
  </Popover>
</template>

<script setup>
import { Popover, PopoverContent, PopoverTrigger } from '~/components/ui/popover'
import { Button } from '~/components/ui/button'
import { ChevronsUpDownIcon } from 'lucide-vue-next'
import { Calendar } from '~/components/ui/calendar'
import { getLocalTimeZone, today, DateFormatter } from '@internationalized/date'

const dateFormatter = new DateFormatter('uk-UA')

defineOptions({
  inheritAttrs: false
})

const emit = defineEmits(['update:modelValue'])

const open = ref(false)
const selectedDate = ref('')
const formattedDate = computed(() => {
  if (selectedDate.value) {
    const date = selectedDate.value.toDate()
    return dateFormatter.format(date)
  } else {
    return 'Select date'
  }
})

const placeholder = today(getLocalTimeZone())

const setSelectedDate = (date) => {
  selectedDate.value = date
  emit('update:modelValue', date)
  open.value = false
}
</script>
