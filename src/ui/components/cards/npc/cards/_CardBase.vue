<template>
  <v-card flat tile class="my-1" color="transparent">
    <v-card-title
      :class="`${item.Feature.Color} sliced pb-1 pt-2`"
      style="max-height:20px; line-height: 0"
    >
      <div
        :class="`heading flavor-text white--text ${active ? 'pt-1 pb-2' : 'py-2 mt-n3'}`"
        style="font-size:20px; line-height: 0"
      >
        <v-menu v-if="!readonly && !active" offset-x left>
          <template v-slot:activator="{ on }">
            <v-icon icon small dark class="fadeSelect mt-n1" v-on="on">mdi-settings</v-icon>
          </template>
          <v-list dense>
            <v-list-item :disabled="item.Tier === 3" @click="item.Tier++">
              <v-list-item-icon class="ma-0 mr-2 mt-3">
                <v-icon>mdi-arrow-up-thick</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>Upgrade Tier</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item :disabled="item.Tier === 1" @click="item.Tier--">
              <v-list-item-icon class="ma-0 mr-2 mt-2">
                <v-icon>mdi-arrow-down-thick</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>Downgrade Tier</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click="$refs.cName.show()">
              <v-list-item-icon class="ma-0 mr-2 mt-2">
                <v-icon>mdi-circle-edit-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>Set Custom Name</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item @click="$refs.cDesc.show()">
              <v-list-item-icon class="ma-0 mr-2 mt-2">
                <v-icon>mdi-circle-edit-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>Set Custom Description</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-divider />
            <v-list-item @click="$emit('remove-feature', $event)">
              <v-list-item-icon class="ma-0 mr-2 mt-2">
                <v-icon color="error">mdi-delete</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>Remove {{ item.Feature.FeatureType }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-menu>
        <span v-if="readonly">
          {{ item.Feature.Name }}
        </span>
        <span v-else :style="item.Destroyed ? 'text-decoration: line-through' : ''">
          {{ item.Name }} (T{{ item.Tier }})
        </span>
        <span v-if="!item.Destroyed && !active" class="caption">//{{ item.Feature.Origin }}</span>
        &emsp;
      </div>
    </v-card-title>
    <v-card-text
      :class="`py-1 px-2 text--text ${item.Destroyed ? 'error lighten-1' : 'stark-panel'}`"
      :style="`border: 1px solid var(--v-${item.Feature.Color}-base)!important`"
    >
      <p
        v-if="item.Description"
        class="flavor-text px-2 grey lighten-2 mb-1"
        v-html="item.Description"
      />
      <slot />
      <div v-if="active">
        <v-divider class="my-1" />
        <v-row dense>
          <slot name="extra-action" />
          <v-col v-if="item.Feature.IsRecharging" class="ml-2 mt-n1">
            <v-switch
              v-model="item.IsCharged"
              :label="item.IsCharged ? 'Charged' : `Recharges on ${item.Feature.ChargeRoll}+`"
              inset
              dense
              hide-details
              color="primary"
              class="mt-0 flavor-text"
            />
          </v-col>
          <v-col v-if="item.MaxUses" class="ml-2 mt-n1">
            <span class="caption">USES:</span>
            <cc-item-uses class="d-inline" :item="item" />
          </v-col>
          <v-col cols="auto" class="ml-auto mr-2 mt-n2">
            <cc-tooltip simple :content="`Mark ${item.Destroyed ? 'Repaired' : 'Destroyed'}`">
              <v-btn
                icon
                :color="item.Destroyed ? 'secondary' : 'error'"
                class="fadeSelect"
                @click="item.Destroyed = !item.Destroyed"
              >
                <v-icon>
                  {{ item.Destroyed ? 'cci-repair' : 'mdi-image-broken-variant' }}
                </v-icon>
              </v-btn>
            </cc-tooltip>
          </v-col>
        </v-row>
        <slot name="active-actions" />
      </div>
    </v-card-text>
    <cc-string-edit-dialog
      v-if="!readonly && !active"
      ref="cName"
      :placeholder="item.Feature.Name"
      label="Custom Item Name"
      @save="item.Name = $event"
      @reset="item.Name = item.Feature.Name"
    />
    <cc-string-edit-dialog
      v-if="!readonly && !active"
      ref="cDesc"
      :placeholder="item.Feature.Description"
      label="Custom Item Description"
      @save="item.Description = $event"
      @reset="item.Description = ''"
    />
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  name: 'npc-item-card-base',
  props: {
    item: {
      type: Object,
      required: true,
    },
    readonly: {
      type: Boolean,
    },
    active: {
      type: Boolean,
    },
  },
})
</script>
