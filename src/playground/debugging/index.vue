<template lang="">
  <n-card title="Debugging">
    <n-tabs
      :default-value="subdirectories[0]"
      size="large"
      justify-content="space-evenly"
    >
      <n-tab-pane :name="subdirectories[0]" :tab="subdirectories[0]" v-for="name in subdirectories">
        <component :is="nextComponent"></component>
      </n-tab-pane>
    </n-tabs>
  </n-card>
</template>
<script>
import { getAllDirectories } from "@/utils/os/getAllDirectories.js"
import { NCard, NTabs, NTabPane } from "naive-ui"
import {defineAsyncComponent} from "vue";

export default {
  data() {
    return {
      subdirectories: [],
      moduleImports: []
    }
  },
  mounted() {
    const modules = import.meta.glob("./**/index.vue")
    for (const [key, value] of Object.entries(modules)) {
        //import { test }
        this.moduleImports.push(key)
    }
    console.log("🚀 ~ file: index.vue ~ line 26 ~ mounted ~ moduleNames", this.moduleImports[0])

    this.subdirectories = getAllDirectories(modules)
    console.log("subdirs", this.subdirectories)
  },
  computed: {
    nextComponent() {
    const modules = import.meta.glob("./**/index.vue")
    var keyVal = ""
    for (const [key, value] of Object.entries(modules)) {
        //import { test }
        keyVal = key
    }
    return defineAsyncComponent(() => import(keyVal))
    }
  }
}
</script>
<style lang=""></style>
