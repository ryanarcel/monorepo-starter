<script setup lang="ts">
 
// const { data, status, refresh } = useFetch<any>('http://localhost:8000/api/posts')
const { data, status, refresh } = useFetch<any>('https://jsonplaceholder.typicode.com/posts')
  
 
const columns = [
  {
    header: 'id',
    accessorKey: 'id',
  },
  {
    header: 'title',
    accessorKey: 'title',
  },
  {
    header: 'Description',
    accessorKey: 'description',
  },
]
 
const pagination = ref({
  pageSize: 5,
  pageIndex: 0,
})
 
const table = useTemplateRef('table')
</script>
 
<template>
  <div>
    <div class="flex my-3 space-x-4">
      <NButton
        @click="$event => refresh()"
        btn="solid-white">Refresh
      </NButton>
      <NButton>Add
      </NButton>

    </div>
    <NTable
      ref="table"
      :columns
      :data="data"
      enable-sorting
      enable-column-filters
      :loading="status === 'pending'"
    />
 
    <!-- pagination -->
    <div
      class="mt-5 flex flex-wrap items-center justify-between gap-4 overflow-auto px-2"
    >
      <div
        class="flex items-center justify-center text-sm font-medium"
      >
        Page {{ (table?.getState().pagination.pageIndex ?? 0) + 1 }} of
        {{ table?.getPageCount().toLocaleString() }}
      </div>
 
      <NPagination
        :page="(table?.getState().pagination.pageIndex ?? 0) + 1"
        :total="table?.getFilteredRowModel().rows.length"
        show-edges
        :items-per-page="table?.getState().pagination.pageSize"
        @update:page="table?.setPageIndex($event - 1)"
      />
    </div>
  </div>
</template>
