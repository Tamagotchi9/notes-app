<template>
  <form class="flex flex-col gap-4" @submit.prevent="submit">
    <base-form-field name="title" placeholder="Title*" />
    <base-form-field name="text" as="textarea" placeholder="Text*" />
    <CategoryAutocomplete v-model="categoryId" :categories="categories" />
    <div class="flex gap-4 items-end">
      <div class="w-2/3 flex flex-col gap-2">
        <Label>Note deadline</Label>
        <CalendarButton v-model="deadlineDate" />
      </div>
      <Input v-model="deadlineTime" type="time" class="w-1/3" />
    </div>
    <Button
      type="submit"
      class="w-30 cursor-pointer hover:scale-103"
      variant="outline"
      :disabled="loading"
    >
      {{ isEdit ? 'Edit note' : 'Add note' }}
    </Button>
  </form>
</template>

<script setup>
import { Button } from '~/components/ui/button'
import { toast } from 'vue-sonner'
import { toTypedSchema } from '@vee-validate/zod'
import { useForm } from 'vee-validate'
import { z } from 'zod'
import { useNotesStore } from '~/store/notes'
import { useCategoriesStore } from '~/store/categories.js'
import { storeToRefs } from 'pinia'
import BaseFormField from '~/components/base-form-field.vue'
import CalendarButton from '~/components/calendar-button.vue'
import { fromDate, getLocalTimeZone, CalendarDateTime } from '@internationalized/date'

const notesStore = useNotesStore()
const categoriesStore = useCategoriesStore()
const { categories, selectedCategoryId } = storeToRefs(categoriesStore)

const props = defineProps({
  note: {
    type: Object,
    default: null
  },
  closeModal: {
    type: Function,
    default: null
  }
})

const isEdit = computed(() => props.note !== null)

const formSchema = toTypedSchema(
  z.object({
    title: z.string().min(1, 'Title is required'),
    text: z.string().min(1, 'Text is required')
  })
)

const { handleSubmit, resetForm, setValues } = useForm({
  validationSchema: formSchema
})

const categoryId = ref(selectedCategoryId.value !== 'all' ? selectedCategoryId.value : null)
const deadlineDate = ref(fromDate(new Date(), getLocalTimeZone()))
const deadlineTime = ref('09:00:00')

const formattedDeadlineAt = computed(() => {
  const year = deadlineDate.value.year
  const month = deadlineDate.value.month
  const day = deadlineDate.value.day
  const hour = deadlineTime.value.split(':')[0]
  const minute = deadlineTime.value.split(':')[1]
  const second = deadlineTime.value.split(':')[2]
  return new CalendarDateTime(year, month, day, hour, minute, second).toString()
})

const loading = ref(false)

const submit = handleSubmit(async (values) => {
  loading.value = true
  try {
    const payload = {
      title: values.title,
      text: values.text,
      category: categoryId.value || null,
      deadlineAt: formattedDeadlineAt.value || null
    }
    if (isEdit.value) {
      await notesStore.update(props.note._id, payload)
    } else {
      await notesStore.create(payload)
    }
    toast.success(isEdit.value ? 'Note has been updated' : 'Note has been created')
    resetForm()
    if (props.closeModal) {
      props.closeModal()
    }
    categoryId.value = null
  } catch (error) {
    toast.error('Failed to create note')
    console.error(error)
  } finally {
    loading.value = false
  }
})

onMounted(() => {
  if (isEdit.value) {
    setValues({
      title: props.note.title,
      text: props.note.text
    })
  }
})
</script>
