<script setup>
import TableSortFilterButton from './TableSortFilterButton.vue'
import rawData from '../assets/data.json'
import { onBeforeMount, ref } from 'vue'

const tableData = ref(rawData)
const componentKey = ref(0)
let statuses = tableData.value.map((c) => c.status)
let createdDates = tableData.value.map((c) => c.created)
const currentSortParam = ref('creative_id')
const isAsc = ref(false)
const sorter = { currentSortParam, isAsc }
let filter

statuses = removeDuplicates(statuses)
createdDates = removeDuplicates(createdDates)

onBeforeMount(() => {
  sortOrderTable(sorter.currentSortParam.value)
})

function sortOrderTable(sortParam) {
  if (sortParam !== sorter.currentSortParam.value) {
    sorter.currentSortParam.value = sortParam
    if (sorter.isAsc.value) {
      sortTable(sortParam)
    } else {
      sortTable(sortParam)
      tableData.value.reverse()
    }
  } else {
    if (sorter.isAsc.value) {
      sorter.isAsc.value = false
      sortTable(sortParam)
      tableData.value.reverse()
    } else {
      sorter.isAsc.value = true
      sortTable(sortParam)
    }
  }
  // console.log('Sort by: ' + currentSortParam.value + ', order ascending: ' + isAsc.value)
  componentKey.value += 1
}

function removeDuplicates(arr) {
  return [...new Set(arr)]
}

function sortTable(sortParam) {
  tableData.value.sort((a, b) => {
    const sortParamA = a[sortParam].toString().toUpperCase()
    const sortParamB = b[sortParam].toString().toUpperCase()
    if (sortParamA < sortParamB) return -1
    else if (sortParamA > sortParamB) return 1
    else return 0
  })
}

function filterTable(filterParam, key) {
  tableData.value = rawData.filter((el) => el[filterParam] == key)
  filter = filterParam
  // console.log('Active filter: ' + filterParam + '=' + key)
  componentKey.value += 1
}

function resetTableFilter() {
  tableData.value = rawData
  filter = null
  // console.log('Filter cleared')
  componentKey.value += 1
}
</script>

<template>
  <div :key="componentKey">
    <div class="sortFilterButtonRow">
      <TableSortFilterButton
        param="status"
        @sort="sortOrderTable"
        :current-sort-param="currentSortParam"
        :is-asc="isAsc"
        filterable
        :filter-dictionary="statuses"
        @filterKey="filterTable"
        :filter="filter"
        @reset="resetTableFilter"
      ></TableSortFilterButton>
      <TableSortFilterButton
        param="name"
        @sort="sortOrderTable"
        :current-sort-param="currentSortParam"
        :is-asc="isAsc"
      ></TableSortFilterButton>
      <TableSortFilterButton
        param="message_id"
        @sort="sortOrderTable"
        :current-sort-param="currentSortParam"
        :is-asc="isAsc"
      ></TableSortFilterButton>
      <TableSortFilterButton
        param="creative_id"
        @sort="sortOrderTable"
        :current-sort-param="currentSortParam"
        :is-asc="isAsc"
      ></TableSortFilterButton>
      <TableSortFilterButton
        param="created"
        @sort="sortOrderTable"
        :current-sort-param="currentSortParam"
        :is-asc="isAsc"
        filterable
        :filter-dictionary="createdDates"
        @filterKey="filterTable"
        :filter="filter"
        @reset="resetTableFilter"
      ></TableSortFilterButton>
    </div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="status" label="Status" width="100" />
      <el-table-column prop="name" label="Name" width="200" />
      <el-table-column prop="message_id" label="Message Id" width="200" />
      <el-table-column prop="creative_id" label="Creative Id" width="100" />
      <el-table-column prop="created" label="Created" width="100" />
    </el-table>
  </div>
</template>

<style scoped>
.sortFilterButtonRow {
  width: 100%;
  display: flex;
  justify-content: space-between;
}
</style>
