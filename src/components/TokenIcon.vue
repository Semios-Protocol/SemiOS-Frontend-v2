<template>
  <span>
    <!-- Token -->
    <a class="token-icon" :href="aPush()" target="_blank" v-if="props.daoToken" @click.stop>
      <Link class="text-none" variant="text" :size="props.btnsize" :style="{
          'font-size': props.size,
          color: props.color,
        }">
        <template v-slot:default>
          <v-tooltip activator="parent" location="top" max-width="300">
            {{ props.daoSymbol }}
          </v-tooltip>

          {{ truncateStringCenter(props.daoSymbol) }}
        </template>
      </Link>
    </a>
    <!-- ETH -->
    <a class="token-icon" :href="aPush()" target="_blank" v-else @click.stop>
      <Link class="text-none" variant="text" :size="props.btnsize" :style="{
          'font-size': props.size,
          color: props.color,
        }">
        <template v-slot:default>
          <v-tooltip activator="parent" location="top" max-width="300">
            {{ props.payCurrencyType }}
          </v-tooltip>
          {{ truncateStringCenter(props.payCurrencyType) }}
        </template>
      </Link>
    </a>
  </span>
</template>

<script lang="ts" setup>
import Link from './Link.vue';
const props = defineProps({
  daoToken: {
    type: Boolean,
    default: false,
  },
  daoSymbol: {
    type: String,
    default: '',
  },
  daoErc20Address: {
    type: String,
    default: '',
  },
  payCurrencyType: {
    type: String,
    default: '',
  },
  inputTokenAddress: {
    type: String,
    default: '',
  },
  color: {
    type: String,
    default: '#8C91FF',
  },
  size: {
    type: String,
    default: '16px',
  },
  btnsize: {
    type: String,
    default: 'small',
  },
})

import { truncateStringCenter } from '@/utils'
import { APP_OPEN_URL } from '@/config'
const aPush = () => {
  if (props.daoToken) {
    return `${APP_OPEN_URL}/address/${props.daoErc20Address}`
  } else {
    return props.inputTokenAddress
      ? `${APP_OPEN_URL}/address/${props.inputTokenAddress}`
      : `${APP_OPEN_URL}/chart/etherprice`
  }
}
</script>

<style lang="scss" scoped>
.token-icon {
  display: flex;
  justify-content: center;
  text-decoration: none;
}
.text-none {
  padding: 0 6px;
}
</style>
