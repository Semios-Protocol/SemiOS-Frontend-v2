<template>
  <FormRow
    :importance="false"
    :input-name="
      $t('AddFormNodesStrategies.whitelistedERC721WithMaxMintingLabel')
    "
    :tooltip-text="
      $t('AddFormNodesStrategies.whitelistedERC721WithMaxMintingTip')
    "
    class="my-mgb12"
  >
    <v-row
      no-gutters
      class="no-def"
      v-if="mintingErcWithMaxlistAddress.length > 0"
    >
      <v-col>
        <v-card
          variant="tonal"
          v-for="(item, idx) in mintingErcWithMaxlistAddress"
          :key="item.nftAddress + idx"
        >
          <v-row no-gutters>
            <v-col cols="10">
              <p>{{ item.nftAddress }}</p>
              <p>{{ t('common.amount') }} : {{ item.nftMintCap }}</p>
            </v-col>
            <v-col cols="2">
              <v-icon color="#745cd4" @click="deleteItem(idx)" class="">
                mdi-trash-can-outline
              </v-icon>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
    <v-btn
      border
      class="text-none btnmo btnb ftr fc7"
      prepend-icon="mdi-plus"
      variant="text"
      block
      size="large"
      style="width: 100%"
      @click="setDialog"
    >
    </v-btn>
  </FormRow>

  <DialogFormMinterWhitelistERCWithMax
    :is-dialog="isDialog"
    @cancelDialog="cancelDialog"
    @addData="addData"
    :data-list="mintingErcWithMaxlistAddress"
    :dialog-title="
      $t('AddFormNodesStrategies.whitelistedERC721WithMaxMintingLabel')
    "
  />
</template>

<script setup lang="ts">
import FormRow from '@/components/FormRow.vue'
import DialogFormMinterWhitelistERCWithMax from '@/components/nodeStrategies/DialogFormMinterWhitelistERCWithMax.vue'
import { t } from '@/lang'
import { ref, onMounted, watch } from 'vue'
const props = defineProps({
  isEdit: {
    type: Boolean,
    default: false,
  },
  initData: {
    type: Object,
    default: () => {},
  },
  list: {
    type: Array<{
      nftAddress: string
      nftMintCap: number
    }>,
    default: () => [],
  },
})
const mintingErcWithMaxlistAddress = ref(props.list)
watch(
  () => props.list,
  (val) => {
    mintingErcWithMaxlistAddress.value = val
  },
  { deep: true }
)

const isDialog = ref(false)
const setDialog = () => {
  isDialog.value = true
}
const addData = (data: never) => {
  mintingErcWithMaxlistAddress.value.push(data)
  setFormData()
}
const deleteItem = (idx: number) => {
  mintingErcWithMaxlistAddress.value.splice(idx, 1)
  setFormData()
}

const cancelDialog = () => {
  isDialog.value = false
}

const emit = defineEmits(['setFormData'])

const setFormData = () => {
  emit(
    'setFormData',
    mintingErcWithMaxlistAddress.value,
    'mintingErcWithMaxlistAddress'
  )
}
onMounted(async () => {
  if (props.isEdit) {
    mintingErcWithMaxlistAddress.value = props.initData.minting?.erc721MintCaps
      ? props.initData.minting.erc721MintCaps.map((item: any) => {
          return {
            nftAddress: item.account,
            nftMintCap: item.cap,
          }
        })
      : []
    setFormData()
  }
})
</script>

<style lang="scss" scoped>
h4 {
  text-align: center;
}
.delete-ipput {
  cursor: pointer;
}
.no-def {
  :deep(.v-card--variant-tonal) {
    background-color: #21293a !important;
    color: #9e9e9e;
    padding: 12px;
    margin-bottom: 6px;
  }
  :deep(.v-col-2) {
    display: flex;
    align-items: center;
    justify-content: flex-end;
  }
}
</style>
