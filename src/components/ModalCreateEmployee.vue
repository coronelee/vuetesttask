<script setup>
const props = defineProps({
  openCloseModal: Function,
  editEmployeeList: Function,
  staffTag: Array,
  position: Array,
  country: Array,
  type_contract: Array,
  gender: Array
})

const createEmployee = () => {
  props.editEmployeeList({
    full_name: document.getElementById('fio').value,
    inn: document.getElementById('inn').value,
    address: document.getElementById('address').value,
    date_birth: document.getElementById('date_birth').value,
    age: document.getElementById('age').value,
    type_contract: document.getElementById('type_contract').value,
    type_contract_id: document.getElementById('type_contract').value,
    gender: document.getElementById('gender_type').value,
    gender_id: document.getElementById('gender_type').value,
    country: document.getElementById('country_type').value,
    country_id: document.getElementById('country_type').value,
    position: document.getElementById('position_type').value,
    position_id: document.getElementById('position_type').value,
    status: document.getElementById('staff').value,
    tag_id: document.getElementById('staff').value,
    tag: document.getElementById('staff').value
  })
}
</script>

<template>
  <div
    class="w-screen h-screen absolute top-0 left-0 backdrop-blur-sm flex justify-center items-center"
  >
    <div class="w-1/3 bg-white p-4 border-2 border-[#DBE4ED]">
      <span class="flex justify-between">
        <span>Добавление нового сотрудника</span>
        <img src="/exit.svg" alt="exit" class="w-8 cursor-pointer" @click="openCloseModal()" />
      </span>
      <div class="flex flex-col gap-4">
        <input type="text" class="w-full" placeholder="ФИО" id="fio" />
        <input type="text" class="w-full" placeholder="ИНН" id="inn" />
        <input type="text" class="w-full" placeholder="Адрес" id="address" />
        <select name="country" id="country_type" class="px-4 py-2">
          <option value="" selected disabled hidden>Страна</option>

          <option v-for="country in country" :key="country.id" :value="country.id">
            {{ country.title }}
          </option>
        </select>
        <select name="type_contract" id="type_contract" class="px-4 py-2">
          <option value="" selected disabled hidden>Тип контракта</option>

          <option v-for="type in type_contract" :key="type.slug" :value="type.id">
            {{ type.slug }}
          </option>
        </select>
        <span>Дата рождения <input type="date" id="date_birth" /></span>
        <input type="text" placeholder="Возраст" id="age" />
        <div class="flex gap-4">
          <span v-for="gender in gender" :key="gender">
            <input type="radio" name="example" :value="gender.id" id="gender_type" />
            {{ gender.title }}
          </span>
        </div>
        <select name="position" id="position_type" class="px-4 py-2">
          <option value="" selected disabled hidden>Должность</option>
          <option v-for="position in position" :key="position" :value="position.id">
            {{ position.name }}
          </option>
        </select>
        <select name="staff" id="staff" class="px-4 py-2">
          <option value="" selected disabled hidden>Категория</option>
          <option
            v-for="staff in staffTag"
            :key="staff"
            :value="staff.id"
            :style="{ 'background-color': staff.bgColor, color: staff.textColor }"
            :v-if="staff.id !== 1"
          >
            {{ staff.title }}
          </option>
        </select>
        <button @click="() => createEmployee()" class="w-full bg-[#2A358C] text-white py-4">
          Создать
        </button>
      </div>
    </div>
  </div>
</template>
