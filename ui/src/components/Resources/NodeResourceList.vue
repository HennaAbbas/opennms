<template>
  <div class="feather-row">
    <div class="feather-col-12">
      <div v-if="resources.length">
        <FeatherButton primary @click="selectAll">Select All</FeatherButton>
        <FeatherButton primary @click="clearAll">Clear All</FeatherButton>
        <FeatherButton primary @click="graphAll">Graph All</FeatherButton>
        <FeatherButton primary @click="graphSelected" :disabled="!resourceIsSelected">Graph Selected</FeatherButton>
      </div>
      <FeatherList>
        <template v-for="(resources, header) in groupedResourcesObject" :key="header">
          <FeatherListHeader>{{ header }}</FeatherListHeader>
          <FeatherListItem v-for="resource in resources" :key="resource.label">
            <FeatherCheckbox
              @update:modelValue="selectCheckbox(resource.id)"
              :modelValue="selectedResourceObject[resource.id]"
            >{{ resource.label }}</FeatherCheckbox>
          </FeatherListItem>
          <FeatherListSeparator />
        </template>
      </FeatherList>
    </div>
  </div>
</template>
  
<script setup lang=ts>
import { useStore } from 'vuex'
import { groupBy } from 'lodash'
import { FeatherCheckbox } from '@featherds/checkbox'
import { FeatherButton } from '@featherds/button'
import {
  FeatherListHeader,
  FeatherListItem,
  FeatherList,
  FeatherListSeparator
} from '@featherds/list'
import { Resource } from '@/types'

interface GroupedResourcesObject {
  [x: string]: Resource[]
}

const store = useStore()
const router = useRouter()

const selectedResourceObject = ref<any>({})

const resources = computed<Resource[]>(() => store.state.resourceModule.nodeResource.children?.resource || [])
const groupedResourcesObject = computed<GroupedResourcesObject>(() => groupBy(resources.value, 'typeLabel'))
const resourceIsSelected = computed<boolean>(() => Object.values(selectedResourceObject.value).includes(true))

const selectCheckbox = (resourceId: string) => selectedResourceObject.value[resourceId] = !selectedResourceObject.value[resourceId]
const selectAll = () => {
  for (const resource of resources.value) {
    selectedResourceObject.value[resource.id] = true
  }
}
const clearAll = () => selectedResourceObject.value = {}
const graphSelected = async () => {
  const selectedIds = []
  for (const key in selectedResourceObject.value) {
    if (selectedResourceObject.value[key]) {
      selectedIds.push(key)
    }
  }
  await store.dispatch('graphModule/getGraphDefinitionsByResourceIds', selectedIds)
  router.push('/resource-graphs/graphs')
}
const graphAll = async () => {
  const resourceIds = resources.value.map((resource) => resource.id)
  await store.dispatch('graphModule/getGraphDefinitionsByResourceIds', resourceIds)
  router.push('/resource-graphs/graphs')
}
</script>
