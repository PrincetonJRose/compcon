<template>
  <sidebar-array-view title="EQUIPMENT TAGS" :array="tags" icon="mdi-tag" />
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import _ from 'lodash'
import SidebarArrayView from '../components/SidebarArrayView.vue'
import { getModule } from 'vuex-module-decorators'
import { CompendiumStore } from '@/store'

@Component({
  components: { SidebarArrayView },
})
export default class Tags extends Vue {
  private compendium = getModule(CompendiumStore, this.$store)
  get tags() {
    return _.sortBy(
      this.compendium.Tags.filter(x => !x.IsHidden),
      'Name'
    )
  }
}
</script>
