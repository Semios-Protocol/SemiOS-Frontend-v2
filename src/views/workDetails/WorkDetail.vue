<template>
  <v-card class="work-card">
    <h4 class="font-h4">Details</h4>
    <div class="card-top">
      <v-row align="start" no-gutters class="card-num">
        <v-col cols="3" class="my-flexaj">
          {{ props.dataObj.canvasCurrentPrice }}
          <TokenIcon
            :daoToken="props.dataObj.erc20PaymentMode"
            :daoSymbol="props.dataObj.daoSymbol"
            :daoErc20Address="props.dataObj.daoErc20Address"
            :payCurrencyType="props.dataObj.payCurrencyType"
            :inputTokenAddress="props.dataObj.inputTokenAddress"
          />
        </v-col>
        <v-col class="my-flexaj" cols="3">
          {{ props.dataObj.daoFloorPrice }}
          <TokenIcon
            :daoToken="props.dataObj.erc20PaymentMode"
            :daoSymbol="props.dataObj.daoSymbol"
            :daoErc20Address="props.dataObj.daoErc20Address"
            :payCurrencyType="props.dataObj.payCurrencyType"
            :inputTokenAddress="props.dataObj.inputTokenAddress"
          />
        </v-col>
        <v-col cols="3">
          <span>
            {{ ellipsis(props.dataObj.workHash) }}
            <v-tooltip activator="parent" location="top">
              {{ props.dataObj.workHash }}
            </v-tooltip>
          </span>
          
          <CopyInformation
            size="36"
            fontSize="16"
            :address="props.dataObj.workHash"
          />
        </v-col>
        <v-col cols="3">
          <a
            class="token-icon"
            :href="openUrl(props.dataObj.creatorAddress)"
            target="_blank"
            v-if="props.dataObj.creatorAddress"
          >
            <v-btn class="text-none a-btn" variant="text">
              <span class="text-style2">
                <span v-if="props.dataObj.creatorName"
                  >{{ ellipsis(props.dataObj.creatorName) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.creatorName }}
                  </v-tooltip></span
                >
                <span v-else
                  >{{ ellipsis(props.dataObj.creatorAddress) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.creatorAddress }}
                  </v-tooltip></span
                >
              </span>
            </v-btn>
          </a>
          <span class="card-name" v-else>Unminted</span>
        </v-col>
      </v-row>
      <v-row align="start" no-gutters class="card-name">
        <v-col cols="3"> Current price </v-col>
        <v-col cols="3"> DAO floor price</v-col>
        <v-col cols="3">Hash </v-col>
        <v-col cols="3"> Create by </v-col>
      </v-row>
    </div>
    <div>
      <v-row align="start" no-gutters class="card-num">
        <v-col class="my-flexaj" cols="3">
          <a
            class="token-icon"
            :href="openUrl(props.dataObj.mintedBy)"
            target="_blank"
            v-if="props.dataObj.mintedBy"
          >
            <v-btn class="text-none a-btn" variant="text">
              <span class="text-style2">
                <span v-if="props.dataObj.mintedName"
                  >{{ ellipsis(props.dataObj.mintedName) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.mintedName }}
                  </v-tooltip>
                </span>
                <span v-else
                  >{{ ellipsis(props.dataObj.mintedBy) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.mintedBy }}
                  </v-tooltip>
                </span>
              </span>
            </v-btn>
          </a>
          <span class="card-name" v-else>Unminted</span>
        </v-col>
        <v-col class="my-flexaj" cols="3">
          <a
            class="token-icon"
            :href="openUrl(props.dataObj.ownerAddress)"
            target="_blank"
            v-if="props.dataObj.ownerAddress"
          >
            <v-btn class="text-none a-btn" variant="text">
              <span class="text-style2">
                <span v-if="props.dataObj.ownerName"
                  >{{ ellipsis(props.dataObj.ownerName) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.ownerName }}
                  </v-tooltip>
                </span>
                <span v-else
                  >{{ ellipsis(props.dataObj.ownerAddress) }}
                  <v-tooltip activator="parent" location="top">
                    {{ props.dataObj.ownerAddress }}
                  </v-tooltip>
                </span>
              </span>
            </v-btn>
          </a>
          <span class="card-name" v-else>Unminted</span>
        </v-col>
        <v-col class="my-flexaj" cols="3">
          <a
            class="token-icon"
            :href="openUrl(props.dataObj.mintedBlock, 'block')"
            target="_blank"
            v-if="props.dataObj.mintedBlock"
          >
            <v-btn class="text-none a-btn" variant="text">
              {{ props.dataObj.mintedBlock }}
            </v-btn>
          </a>
          <span class="card-name" v-else> Unminted </span>
        </v-col>
        <v-col class="my-flexaj" cols="3">
          <span class="card-name text-white" v-if="props.dataObj.mintedDate">
            {{ workTimeFormat(props.dataObj.mintedDate) }}
          </span>
          <span class="card-name text-white" v-else> Unminted </span>
        </v-col>
      </v-row>
      <v-row align="start" no-gutters class="card-name">
        <v-col cols="3">Minted by </v-col>
        <v-col cols="3">Owned by </v-col>
        <v-col cols="3">Minted Block </v-col>
        <v-col cols="3">Minted Date </v-col>
      </v-row>
    </div>
  </v-card>
</template>

<script setup lang="ts">
import TokenIcon from '@/components/TokenIcon.vue'
import CopyInformation from '@/components/CopyInformation.vue'
import { ellipsis, workTimeFormat } from '@/utils'
const props = defineProps({
  dataObj: {
    type: Object,
    default: () => {},
  },
})
import { APP_OPEN_URL } from '@/config'
const openUrl = (address: string, type: string = 'address') => {
  return `${APP_OPEN_URL}/${type}/${address}`
}
</script>
<style scoped lang="scss">
.work-card {
  background-color: #1A1F2E !important;
}
.card-top {
  margin-bottom: 16px;
}
.card-num {
  color: #fff;
  font-size: 16px;
}
.card-name {
  color: #9e9e9e;
  font-size: 16px;
  font-weight: 400;
}
.a-style {
  color: #b3b5f2 !important;
}
.my-flexaj {
  line-height: 36px;
}
.a-btn {
  @apply text-primary-1;
  padding: 0 6px;
  font-size: 16px;
  margin-left: -6px;
  min-width: auto;
}
</style>
