<template>
  <equipment-card-base :item="item">
    <v-col cols="auto" class="ml-auto text-right">
      <div class="heading h2">{{ item.FeatureType }}</div>
      <span class="flavor-text grey--text">// {{ item.Origin }}</span>
    </v-col>
  </equipment-card-base>
</template>

<script lang="ts">
import Vue from 'vue'
import EquipmentCardBase from './_EquipmentCardBase.vue'

export default Vue.extend({
  name: 'cc-npc-system-card',
  components: { EquipmentCardBase },
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
})
</script>
