<template>
  <div class="card">
    <div class="feather-row">
      <div class="feather-col-12">
        <div class="dcb-container">
          <div class="table-container">
            <p class="title">Device Configuration</p>
            <DCBTable />
          </div>
          <div v-if="false" class="filters-container">
            <DCBGroupFilters />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useStore } from 'vuex'
import DCBTable from '@/components/Device/DCBTable.vue'
import DCBGroupFilters from '@/components/Device/DCBGroupFilters.vue'

const store = useStore()

onMounted(() => store.dispatch('deviceModule/getDeviceConfigBackups'))
</script>

<style scoped lang="scss">
@import "@featherds/styles/mixins/elevation";
@import "@featherds/styles/mixins/typography";

@mixin status-bar($color) {
  background: $color;
  background: linear-gradient(90deg, $color 1%, rgba(255, 255, 255, 0) 9%);
}
:deep(.success) {
  @include status-bar(var($success));
}
:deep(.failed) {
  @include status-bar(var($error));
}
:deep(.none) {
  @include status-bar(var($shade-4));
}

.card {
  @include elevation(2);
  background: var($surface);
  padding: 10px 20px 20px 20px;

  .dcb-container {
    display: flex;

    .table-container {
      width: 35rem;
      flex: auto;
      .title {
        @include headline4;
        margin-left: 19px;
      }
    }

    .filters-container {
      width: 15rem;
    }
  }
}
</style>
