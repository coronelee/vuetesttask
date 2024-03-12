<script setup>
import { ref, onMounted } from 'vue'
const props = defineProps({
  editFilterCategoryList: Function,
  filterCategoryList: String,
  employeeList: Array,
  staffTag: Array,
  position: Array,
  country: Array,
  type_contract: Array,
  gender: Array
})
onMounted(() => {
  for (let i = 0; i < props.staffTag.length; i++) {
    document.getElementById(i + 1).style.border = `1px solid ${props.staffTag[i].textColor}`
    document.getElementById(i + 1).style.backgroundColor = props.staffTag[i].bgColor
    document.getElementById(i + 1).style.color = props.staffTag[i].textColor
  }
})
</script>

<template>
  <div class="bg-white w-2/3">
    <div class="w-full p-10 border-b border-[#DBE4ED] flex flex-col gap-2">
      <input
        type="text"
        class="w-full bg-[#E0EBEF] px-4 py-4 rounded bg-[url('/search.svg')] bg-no-repeat bg-right text-[#84909B]"
        placeholder="Поиск сотрудника"
      />
      <span class="text-[#B0BCC7]">Например: Иванов Иван</span>
    </div>
    <div class="p-10 flex flex-col gap-6">
      <span class="text-[#041423] text-2xl font-semibold">Список сотрудников</span>
      <div class="flex gap-4">
        <span
          v-for="category in staffTag"
          :key="category.id"
          class="px-4 py-2 rounded-full font-semibold cursor-pointer"
          :id="category.id"
          @click="editFilterCategoryList($event.target.id)"
        >
          {{ category.title }}
        </span>
      </div>
      <div class="flex flex-col gap-4">
        <div v-for="employee in employeeList" :key="employee.id" class="w-full">
          <div
            v-if="filterCategoryList == 1 || employee.tag_id == filterCategoryList"
            class="w-full flex flex-col gap-2 items-start rounded bg-[#E7F3FF] w-full p-8"
          >
            <div class="flex flex-wrap gap-4 items-center">
              <span class="text-[#2A358C] font-semibold text-lg">{{ employee.full_name }}</span>
              <span class="bg-white p-1 text-[#84909B]">ИНН: {{ employee.inn }}</span>
              <span class="bg-[#00AE5B] text-white font-bold p-1 rounded">{{
                type_contract[employee.type_contract_id - 1].slug
              }}</span>
              <span class="lowercase">{{ position[employee.position_id - 1].name }}</span>
            </div>
            <div
              class="flex flex-wrap items-center gap-4 [&>span]:border-l [&>span]:border-[#CEDAE5] [&>span]:pl-4"
            >
              <div class="flex gap-2 items-center">
                <img :src="country[employee.country_id - 1].icon" alt="flag" class="w-6" />
                <span class="">
                  {{ country[employee.country_id - 1].slug }}
                </span>
              </div>
              <span>{{ employee.address }}</span>
              <span>Дата рождения: {{ employee.date_birth }}</span>
              <span>Возраст: {{ employee.age }} год</span>
              <span>Пол: {{ gender[employee.gender_id - 1].title }}</span>
            </div>
            <span
              class="text-white px-2 py-1 rounded w-auto mt-4"
              :style="{ 'background-color': staffTag[employee.tag_id - 1].textColor }"
              >{{ staffTag[employee.tag_id - 1].title }}</span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
