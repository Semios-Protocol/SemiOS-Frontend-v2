<template>
  <div class="other-nodes">
    <SlotDialog
      title="Lock Duration"
      :isDialog="props.isDialog"
      @cancelDialog="cancelDialog"
    >
      <v-form ref="formRef">
        <v-text-field
          label="Lock Duration"
          density="comfortable"
          type="number"
          v-model="lockTime"
          :rules="valueRules"
          @update:modelValue="setInput(lockTime, 0, 1, 720)"
        >
         <template v-slot:append-inner>
           <div class="z-10 text-grey-1">{{ t('common.hours') }}</div>
          </template>
        </v-text-field>
      </v-form>
      <v-btn
        block
        class="btnz text-none my-mgt16"
        type="submit"
        @click="lockNft"
        >Lock</v-btn
      >
    </SlotDialog>
  </div>
</template>
<script lang="ts" setup>
import SlotDialog from '@/components/SlotDialog.vue'
import { t } from '@/lang'

import { ref, watch } from 'vue'
const props = defineProps({
  isDialog: {
    type: Boolean,
    default: false,
  },
  dialogTitle: {
    type: String,
    default: '',
  },
})
watch(
  () => props.isDialog,
  (value) => {
    if (value) {
      lockTime.value = ''
    }
  }
)

const valueRules = ref([(v: any) => !!v || 'Item is required'])

const emit = defineEmits(['cancelDialog', 'lockNft'])
const cancelDialog = () => {
  emit('cancelDialog')
}

const lockTime = ref<any>('')

import { oninputNum } from '@/utils'

const setInput = (
  val: string | number,
  position = 0,
  min = 0,
  max = Infinity
) => {
  lockTime.value = oninputNum(val, position, min, max)
}
const formRef = ref()
const lockNft = async () => {
  const { valid } = await formRef.value.validate()
  if (!valid) return false
  emit('lockNft', lockTime.value)
  // cancelDialog()
}
</script>

<style lang="scss" scoped>
:deep(.v-card-actions .v-form) {
  width: 100%;
}
.other-nodes {
  :deep(.v-form) {
    width: 100%;
    background-color: transparent;
  }
}
</style>
