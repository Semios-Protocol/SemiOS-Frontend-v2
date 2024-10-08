<template>
  <div class="other-nodes">
    <SlotDialog
      :title="props.dialogTitle"
      :isDialog="props.isDialog"
      @cancelDialog="cancelDialog"
    >
      <v-form ref="formRef">
        <v-text-field
          :label="$t('NodeDetail.addressPlaceholder')"
          density="comfortable"
          v-model="formData.nftAddress"
          :rules="nftAddressRules"
        >
        </v-text-field>
        <v-text-field
          :label="$t('NodeDetail.nftMintCapPlaceholder')"
          type="number"
          density="comfortable"
          v-model="formData.nftMintCap"
          @update:modelValue="setInput(formData.nftMintCap, 'nftMintCap', 0, 1)"
          :rules="nftMintCapRules"
        >
        </v-text-field>
      </v-form>
      <v-btn
        block
        class="btnz text-none my-mgt16"
        type="submit"
        @click="addData()"
        >Add</v-btn
      >
    </SlotDialog>
  </div>
</template>
<script lang="ts" setup>
import SlotDialog from '@/components/SlotDialog.vue'

import { reactive, ref, watch } from 'vue'
const props = defineProps({
  isDialog: {
    type: Boolean,
    default: false,
  },
  dialogTitle: {
    type: String,
    default: '',
  },
  dataList: {
    type: Array,
    default: () => [],
  },
})
watch(
  () => props.isDialog,
  (value) => {
    if (value) {
      formData.nftAddress = ''
      formData.nftMintCap = undefined
    }
  }
)
const formData = reactive({
  nftAddress: '',
  nftMintCap: undefined,
})

import useIsInput from '@/hooks/useIsInput'
const { isERC721Existence } = useIsInput()
const nftAddressRules = [
  (v: any) => !!v || 'Item is required',
  async (val: any) => {
    return isERC721Existence(val, props.dataList, 'nftAddress')
  },
]
const nftMintCapRules = [(v: any) => !!v || 'Item is required']

import { oninputNum } from '@/utils'
const setInput = (
  val: any,
  type: keyof typeof formData,
  position = 0,
  min = 0,
  max = Infinity
) => {
  const inputNum = oninputNum(val, position, min, max)
  formData[type] = inputNum as never
}
const emit = defineEmits(['cancelDialog', 'addData'])
const cancelDialog = () => {
  emit('cancelDialog')
}

const formRef = ref()
const addData = async () => {
  const { valid } = await formRef.value.validate()
  if (!valid) return false
  emit('addData', { ...formData })
  cancelDialog()
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
