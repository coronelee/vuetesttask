<script setup>
import { ref } from 'vue'
const props = defineProps({
  editFilterCategoryList: Function,
  filterCategoryList: String,
  employeeList: Array,
  staffTag: Array,
  position: Array,
  country: Array,
  type_contract: Array,
  gender: Array,
  editFilteredSearch: Function,
  filteredSearch: String,
  filterComponent: Array,
  openCloseEditModal: Function
})
const maxLength = ref(4)
const selectFilterItems = (e) => {
  if (e.target.id != 1) {
    if (e.target.style.backgroundColor === props.staffTag[e.target.id - 1].rgbBg) {
      e.target.style.backgroundColor = props.staffTag[e.target.id - 1].textColor
      e.target.style.color = props.staffTag[e.target.id - 1].bgColor
    } else {
      e.target.style.backgroundColor = props.staffTag[e.target.id - 1].bgColor
      e.target.style.color = props.staffTag[e.target.id - 1].textColor
    }
  }
}

const filterSearch = ref('')
const filterEmployee = (value) => {
  filterSearch.value = value
}
</script>

<template>
  <div class="bg-white w-2/3">
    <div class="w-full p-10 border-b border-[#DBE4ED] flex flex-col gap-2">
      <input
        type="text"
        class="w-full bg-[#E0EBEF] px-4 py-4 rounded bg-[url('/search.svg')] bg-no-repeat bg-[99%] text-[#84909B]"
        placeholder="Поиск сотрудника"
        @input="filterEmployee($event.target.value)"
      />
      <span class="text-[#B0BCC7]">Например: Иванов Иван</span>
    </div>
    <div class="p-10 flex flex-col gap-6 items-start">
      <span class="text-[#041423] text-2xl font-semibold">Список сотрудников</span>
      <div class="flex gap-4 flew-wrap">
        <span
          v-for="category in staffTag"
          :key="category.id"
          class="px-4 py-2 rounded-full font-semibold cursor-pointer transition-all duration-500"
          :id="category.id"
          :style="`background-color: ${category.bgColor}; color: ${category.textColor}; border: 1px solid ${category.textColor}`"
          @click="editFilterCategoryList($event.target.id), selectFilterItems($event)"
        >
          {{ category.title }}
        </span>
      </div>
      <div class="flex flex-col gap-4 w-full">
        <div v-for="employee in employeeList.slice(0, maxLength)" :key="employee.id" class="w-full">
          <div
            v-if="
              employee.full_name.split(' ')[0].toLowerCase().includes(filterSearch.toLowerCase()) &&
              (filterCategoryList.includes(employee.tag_id) || filterCategoryList.length === 0) &&
              (filterComponent.country_id == '' ||
                filterComponent.country_id == employee.country_id) &&
              (filterComponent.gender == '' || filterComponent.gender == employee.gender_id) &&
              (filterComponent.position == '' ||
                filterComponent.position == employee.position_id) &&
              ((!filterComponent.type_contractTD &&
                !filterComponent.type_contractGPH &&
                !filterComponent.type_contractSMZ &&
                !filterComponent.type_contractKD) ||
                (filterComponent.type_contractTD && employee.type_contract_id == 1) ||
                (filterComponent.type_contractGPH && employee.type_contract_id == 2) ||
                (filterComponent.type_contractSMZ && employee.type_contract_id == 3) ||
                (filterComponent.type_contractKD && employee.type_contract_id == 4))
            "
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
            <div class="flex flex-wrap items-center justify-center gap-4 mt-4">
              <span
                class="text-white px-2 py-1 rounded w-auto"
                :style="{ 'background-color': staffTag[employee.tag_id - 1].textColor }"
                >{{ staffTag[employee.tag_id - 1].title }}
              </span>
              <button
                class="bg-[#f7f8f9] p-1 rounded drop-shadow-lg hover:drop-shadow-none transition-all duration-600"
                @click="openCloseEditModal(employee)"
              >
                <img src="/edit.svg" alt="edit" class="w-6" />
              </button>
            </div>
          </div>
        </div>
      </div>
      <button
        v-if="employeeList.length >= maxLength"
        class="flex justify-center items-center gap-2 px-4 py-2 text-[#2A358C] border border-[#2A358C] rounded m-auto"
        @click="maxLength += 4"
      >
        <img src="/refresh.svg" alt="refresh" class="w-[18px]" /><span>Показать еще</span>
      </button>
    </div>
  </div>
</template>
