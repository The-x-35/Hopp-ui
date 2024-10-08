<template>
  <Story title="Table">
    <Variant title="General">
      <HoppSmartTable
        :headings="headings"
        :loading="loading"
        :list="list"
        :selected-rows="selectedRows"
      />
    </Variant>

    <!-- Custom implementation of the Table -->
    <Variant title="Custom">
      <HoppSmartTable :loading="loading" :list="list">
        <template #head>
          <th
            v-for="heading in headings"
            :key="heading.key"
            scope="col"
            class="px-6 py-3"
          >
            {{ heading.label }}
          </th>
        </template>

        <template #loading-state>
          <td :colspan="headings.length">
            <div class="mx-auto my-3 h-5 w-5 text-center">
              <HoppSmartSpinner />
            </div>
          </td>
        </template>

        <template #body="{ row }">
          <td
            v-for="cellHeading in headings"
            :key="cellHeading.key"
            class="max-w-[10rem] py-1 pl-6"
          >
            {{ row[cellHeading.key] ?? "-" }}
          </td>
        </template>

        <template #empty-state>
          <td :colspan="headings.length" class="py-3 text-center">
            <p>No data available</p>
          </td>
        </template>
      </HoppSmartTable>
    </Variant>

    <!-- Extending the Table functionality -->
    <Variant title="Extension">
      <HoppSmartTable
        :headings="headings"
        :loading="loading"
        :list="extensionList"
        :selected-rows="selectedRows"
        :sort="{ key: 'name', direction: sortDirection }"
      >
        <template #extension>
          <div class="flex">
            <div class="flex w-full items-center bg-primary">
              <icon-lucide-search class="mx-3 text-xs" />
              <HoppSmartInput
                v-model="searchQuery"
                styles="w-full bg-primary py-1"
                input-styles="h-full border-none"
                placeholder="Search.."
              />
            </div>
            <HoppButton
              :icon="IconArrowUpDown"
              label="Sort"
              class="rounded-none"
              @click="toggleSortDirection"
            />
          </div>
        </template>
      </HoppSmartTable>
    </Variant>
  </Story>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, Ref } from "vue"
import { CellHeading, Direction } from "~/components/smart/Table.vue"
import IconArrowUpDown from "~icons/lucide/arrow-up-down"
import { HoppButton, HoppSmartInput } from ".."
import { HoppSmartSpinner, HoppSmartTable } from "../components/smart"

type List = {
  id: string
  name: string
  members: number
  role: string
}

// Table Headings
const headings: CellHeading[] = [
  { key: "id", label: "ID" },
  { key: "name", label: "Name" },
  { key: "members", label: "Members" },
  { key: "role", label: "Role" },
]

const loading = ref(false)

const selectedRows = ref<List[]>([])

const list: List[] = [
  {
    id: "123456",
    name: "Walter",
    members: 12,
    role: "Chemical Engineer",
  },
  {
    id: "123455",
    name: "Jesse",
    members: 10,
    role: "Lab Assistant",
  },
]

onMounted(async () => {
  loading.value = true

  // Simulate network call
  await new Promise((resolve) => setTimeout(resolve, 1000))

  loading.value = false
})

const sortDirection: Ref<Direction> = ref("ascending")

const toggleSortDirection = () => {
  sortDirection.value =
    sortDirection.value === "ascending" ? "descending" : "ascending"
}

const searchQuery = ref("")

const extensionList = computed(() => {
  return list.filter((item) => {
    return Object.values(item).some((value) =>
      value
        .toString()
        .toLowerCase()
        .includes((searchQuery.value as string).toLowerCase()),
    )
  })
})
</script>
