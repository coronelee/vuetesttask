<script setup>
import { c } from 'vite/dist/node/types.d-FdqQ54oU'
import { onMounted } from 'vue'

const props = defineProps({
  openCloseModal: Function,
  editFilterCategoryList: Function,
  position: Array,
  country: Array,
  type_contract: Array,
  gender: Array,
  editFilterComponent: Function
})

const setFilterItems = (action) => {
  if (action == 'clear') {
    props.editFilterComponent({
      country_id: '',
      gender: '',
      position: '',
      type_contractTD: false,
      type_contractGPH: false,
      type_contractSMZ: false,
      type_contractKD: false
    })
    document.getElementById('countryName').selectedIndex = 0
    document.getElementById('genderName').selectedIndex = 0
    document.getElementById('positionName').selectedIndex = 0
    document.getElementById('ТД').checked = false
    document.getElementById('ГПХ').checked = false
    document.getElementById('СМЗ').checked = false
    document.getElementById('КД').checked = false
  } else if (action == 'set') {
    props.editFilterComponent({
      country_id: document.getElementById('countryName').value,
      gender: document.getElementById('genderName').value,
      position: document.getElementById('positionName').value,
      type_contractTD: document.getElementById('ТД').checked,
      type_contractGPH: document.getElementById('ГПХ').checked,
      type_contractSMZ: document.getElementById('СМЗ').checked,
      type_contractKD: document.getElementById('КД').checked
    })
  }
}
onMounted(() => {
  setFilterItems('clear')
})
</script>

<template>
  <div class="bg-white w-1/3">
    <div class="p-10 border-b border-[#DBE4ED]">
      <button
        class="w-full h-16 bg-[#00B6ED] px-4 py-4 rounded flex justify-center items-center text-white gap-2 drop-shadow-md shadow-[#00B6ED33] shadow-xl"
        @click="openCloseModal()"
      >
        <img src="/create.svg" alt="" />
        <span>Добавить нового сотрудника</span>
      </button>
    </div>
    <div class="p-10 flex flex-col gap-6">
      <span class="text-3xl font-semibold">Фильтр</span>
      <div class="grid grid-cols-2 gap-4 [&>select]:text-[#84909B] [&>select]:bg-[#E8F1F4]">
        <span>Гражданство</span> <span>Пол</span>
        <select name="" id="countryName" class="px-4 py-4">
          <option value="" selected disabled hidden>Все страны</option>
          <option v-for="country in country" :key="country.id" :value="country.id">
            {{ country.title }}
          </option>
        </select>
        <select name="" id="genderName" class="px-4 py-4">
          <option value="" selected disabled hidden>Без разницы</option>
          <option v-for="gender in gender" :key="gender.id" :value="gender.id">
            {{ gender.title }}
          </option>
        </select>
      </div>
      <span>Должность</span>
      <select name="" id="positionName" class="px-4 py-4 text-[#84909B] bg-[#E8F1F4]">
        <option value="" selected disabled hidden>Все должности</option>
        <option v-for="position in position" :key="position.id" :value="position.id">
          {{ position.name }}
        </option>
      </select>
      <div class="flex flex-col gap-1">
        <span>Тип договора</span>
        <span v-for="type in type_contract" :key="type.id" class="flex items-center gap-2">
          <input
            type="checkbox"
            :id="type.slug"
            class="w-5 h-5 border-2 border-[#DCDCDC] appearance-none checked:bg-[url('/check.svg')] checked:bg-no-repeat checked:bg-center checked:bg-[length:17px] cursor-pointer"
          />

          {{ type.title }}
        </span>
      </div>
      <div
        class="border-t border-[#DBE4ED] pt-4 flex justify-between gap-4 items-center [&>button]:w-1/2 [&>button]:h-12 [&>button]:text-white [&>button]:rounded"
      >
        <button class="bg-[#00AE5B]" @click="setFilterItems('set')">Применить</button>
        <button class="bg-[#84909B]" @click="setFilterItems('clear')">Очистить</button>
      </div>
    </div>
  </div>
</template>
