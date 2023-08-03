<script setup>
import { ArrowDown, Top, Bottom } from '@element-plus/icons-vue'

defineProps({
  param: String,
  currentSortParam: String,
  isAsc: Boolean,
  filterable: { type: Boolean, default: false },
  filterDictionary: Array,
  filter: String
})

function prettyName(param) {
  const noUnderscore = param.replace('_', ' ')
  const arr = noUnderscore.split(' ')
  for (var i = 0; i < arr.length; i++) {
    arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1)
  }
  const firstLetterCapital = arr.join(' ')
  return firstLetterCapital
}
</script>

<template>
  <div>
    <el-button @click="$emit('sort', param)">
      {{ prettyName(param) }}
      <el-icon v-if="currentSortParam === param"
        ><top v-if="isAsc" /><bottom v-if="!isAsc"
      /></el-icon>
    </el-button>
    <el-dropdown v-if="filterable">
      <el-button>
        <el-icon><arrow-down /></el-icon>
      </el-button>
      <template #dropdown>
        <el-dropdown-menu>
          <el-dropdown-item
            v-for="item in filterDictionary"
            :key="item"
            @click="$emit('filterKey', param, item)"
            >{{ item }}</el-dropdown-item
          >
          <el-button v-if="filter" @click="$emit('reset')">Reset</el-button>
        </el-dropdown-menu>
      </template>
    </el-dropdown>
  </div>
</template>
