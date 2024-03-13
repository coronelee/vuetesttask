<script setup>
import { onMounted, ref, watch } from 'vue'

const props = defineProps({
  openCloseEditModal: Function,
  valueEdit: Array,
  staffTag: Array,
  position: Array,
  country: Array,
  type_contract: Array,
  gender: Array,
  editEmployeeItem: Function
})
let dateSplit = ref([])
let dateResult = ref([])
let date = ref()
let currentDate
let err = false
onMounted(() => {
  let todayDate = new Date()
  currentDate =
    String(todayDate.getFullYear()) +
    '-' +
    String(todayDate.getMonth() + 1).padStart(2, '0') +
    '-' +
    String(todayDate.getDate()).padStart(2, '0')

  if (props.valueEdit.gender_id == 1) {
    document.getElementById('gender_M').checked = true
  } else {
    document.getElementById('gender_F').checked = true
  }

  date.value = props.valueEdit.date_birth

  dateSplit.value = date.value.split('.')

  dateResult.value = dateSplit.value[2] + '-' + dateSplit.value[1] + '-' + dateSplit.value[0]

  if (dateResult.value[0] == 'u') {
    dateSplit.value = date.value.split('-')
    dateResult.value = dateSplit.value[0] + '-' + dateSplit.value[1] + '-' + dateSplit.value[2]
  }
  console.log(dateResult.value)
})

const editEmployee = () => {
  let gender = 0

  if (document.getElementById('gender_M').checked) {
    gender = 1
  } else {
    gender = 2
  }

  let idFields = [
    'fio',
    'inn',
    'address',
    'date_birth',
    'age',
    'type_contract',
    'country_type',
    'position_type',
    'staff'
  ]

  for (let i = 0; i < idFields.length; i++) {
    console.log(document.getElementById(idFields[i]).value)
  }
  for (let i = 0; i < idFields.length; i++) {
    if (String(document.getElementById(idFields[i]).value) == '') {
      document.getElementById(idFields[i]).classList.add('bg-red-200/25')
    } else {
      document.getElementById(idFields[i]).classList.remove('bg-red-200/25')
      err = false
    }
  }
  for (let i = 0; i < idFields.length; i++) {
    if (document.getElementById(idFields[i]).classList.contains('bg-red-200/25')) {
      err = true
    }
  }
  if (!err) {
    props.editEmployeeItem(props.valueEdit.full_name, [
      document.getElementById('fio').value,
      document.getElementById('inn').value,
      document.getElementById('address').value,
      document.getElementById('date_birth').value,
      document.getElementById('age').value,
      document.getElementById('type_contract').value,
      document.getElementById('type_contract').value,
      gender,
      gender,
      document.getElementById('country_type').value,
      document.getElementById('country_type').value,
      document.getElementById('position_type').value,
      document.getElementById('position_type').value,
      document.getElementById('staff').value,
      document.getElementById('staff').value,
      document.getElementById('staff').value
    ])
  }
}
</script>

<template>
  <div
    class="w-screen h-screen absolute top-0 left-0 backdrop-blur-sm flex justify-center items-center"
  >
    <div class="w-2/5 bg-white p-4 border-2 border-[#DBE4ED]">
      <span class="flex justify-between">
        <span>Изменение данных сотрудника</span>
        <img src="/exit.svg" alt="exit" class="w-8 cursor-pointer" @click="openCloseEditModal()" />
      </span>
      <div
        class="flex flex-col gap-4 [&>input]:w-full [&>select]:w-full [&>input]:px-4 [&>select]:px-4 [&>input]:py-2 [&>select]:py-2"
      >
        <input type="text" id="fio" :value="valueEdit.full_name" />
        <input type="text" id="inn" :value="valueEdit.inn" />
        <input type="text" :value="valueEdit.address" id="address" />
        <select name="country" id="country_type">
          <option :value="valueEdit.country_id" selected disabled hidden>
            {{ country[valueEdit.country_id - 1].title }}
          </option>

          <option v-for="country in country" :key="country.id" :value="country.id">
            {{ country.title }}
          </option>
        </select>
        <select name="type_contract" id="type_contract">
          <option :value="valueEdit.type_contract_id" selected disabled hidden>
            {{ type_contract[valueEdit.type_contract_id - 1].title }}
          </option>

          <option v-for="type in type_contract" :key="type.slug" :value="type.id">
            {{ type.slug }}
          </option>
        </select>
        <span
          >Дата рождения
          <input
            type="date"
            id="date_birth"
            :value="dateResult"
            min="1900-01-01"
            :max="currentDate"
        /></span>
        <input
          type="number"
          placeholder="Возраст"
          id="age"
          class="appearance-none"
          :value="valueEdit.age"
        />
        <div class="flex gap-4">
          <span>
            <input type="radio" name="example" value="1" id="gender_M" />
            Мужской
            <input type="radio" name="example" value="2" id="gender_F" />
            Женский
          </span>
        </div>
        <select name="position" id="position_type">
          <option :value="valueEdit.position_id" selected disabled hidden>
            {{ position[valueEdit.position_id - 1].name }}
          </option>
          <option v-for="position in position" :key="position" :value="position.id">
            {{ position.name }}
          </option>
        </select>
        <select name="staff" id="staff">
          <option :value="valueEdit.tag_id" selected disabled hidden>
            {{ staffTag[valueEdit.tag_id - 1].title }}
          </option>
          <option
            v-for="staff in staffTag"
            :key="staff"
            :value="staff.id"
            :style="{ 'background-color': staff.bgColor, color: staff.textColor }"
          >
            {{ staff.title }}
          </option>
        </select>
        <button
          @click="editEmployee()"
          class="w-full bg-[#13273d] text-white py-4 hover:bg-[#2A358C] transition-all duration-500"
        >
          Изменить
        </button>
      </div>
    </div>
  </div>
</template>
