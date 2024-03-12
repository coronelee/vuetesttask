<script setup>
import EmployeeListComponent from './components/EmployeeListComponent.vue'
import CreateAndFilterEmployeeSheetComponent from './components/CreateAndFilterEmployeeSheetComponent.vue'
import ModalCreateEmployee from './components/ModalCreateEmployee.vue'
import { ref, onMounted } from 'vue'

const staffTag = [
  {
    id: 1,
    bgColor: 'rgba(176, 188, 199, 1)',
    textColor: 'rgba(255, 255, 255, 1)',
    title: 'Весь список'
  },
  {
    id: 2,
    bgColor: 'rgba(255, 253, 241, 1)',
    textColor: 'rgba(226, 189, 6, 1)',
    title: 'Проблемные',
    description: 'Истекает патент'
  },
  {
    id: 3,
    bgColor: 'rgba(255, 248, 248, 1)',
    textColor: 'rgba(229, 46, 46, 1)',
    title: 'Критические',
    description: 'Истекают все документы'
  },
  {
    id: 4,
    bgColor: 'rgba(243, 252, 255, 1)',
    textColor: 'rgba(0, 182, 237, 1)',
    title: 'Есть замечания',
    description: 'Пропустил медосмотр'
  },
  {
    id: 5,
    bgColor: 'rgba(242, 255, 249, 1)',
    textColor: 'rgba(0, 174, 91, 1)',
    title: 'Выполнено',
    description: 'Прошел все процедуры'
  }
]

const country = [
  {
    id: 1,
    icon: '/ru.svg',
    title: 'Россия',
    slug: 'RU'
  },
  {
    id: 2,
    icon: '/uzb.svg',
    title: 'Узбекистан',
    slug: 'UZB'
  },
  {
    id: 3,
    icon: '/tzh.svg',
    title: 'Таджикистан',
    slug: 'TJ'
  }
]
const position = [
  {
    id: 1,
    name: 'Промышленный альпинист'
  },
  {
    id: 2,
    name: 'Токарь'
  },
  {
    id: 3,
    name: 'Пекарь'
  }
]

const type_contract = [
  {
    id: 1,
    title: 'Трудовой договор',
    slug: 'ТД'
  },
  {
    id: 2,
    title: 'Договор гражданско-правового характера',
    slug: 'ГПХ'
  },
  {
    id: 3,
    title: 'Самозанятый',
    slug: 'СМЗ'
  },
  {
    id: 4,
    title: 'Кандидат',
    slug: 'КД'
  }
]

const gender = [
  {
    id: 1,
    title: 'Мужской',
    slug: 'М'
  },
  {
    id: 2,
    title: 'Женский',
    slug: 'Ж'
  }
]

const employeeList = ref([
  {
    full_name: 'Константинопольский Константин Константинович',
    inn: '1234567890',
    address: 'г. Санкт-Петербург',
    date_birth: '23.06.2001',
    age: 21,
    type_contract: 'СМЗ',
    type_contract_id: type_contract[2].id,
    gender: 'Мужской',
    gender_id: gender[0].id,
    country: 'Россия',
    country_id: country[0].id,
    position: 'промышленный альпинист',
    position_id: position[0].id,
    status: staffTag[2].description,
    tag_id: staffTag[2].id,
    tag: 'Критические'
  }
])

const editEmployeeList = (value) => {
  employeeList.value.unshift(value)
  console.log(value)
}
const modalCreateEmployee = ref(false)

const openCloseModal = () => {
  modalCreateEmployee.value = !modalCreateEmployee.value
}

const filterCategoryList = ref(1)

const editFilterCategoryList = (value) => {
  filterCategoryList.value = value
}
</script>

<template>
  <div class="bg-[#f7f8f9] w-screen p-12 flex justify-between items-start gap-12 font-Montserrat">
    <EmployeeListComponent
      :employeeList="employeeList"
      :staffTag="staffTag"
      :position="position"
      :country="country"
      :type_contract="type_contract"
      :gender="gender"
      :editFilterCategoryList="editFilterCategoryList"
      :filterCategoryList="filterCategoryList"
    />
    <CreateAndFilterEmployeeSheetComponent
      :openCloseModal="openCloseModal"
      :country="country"
      :gender="gender"
      :position="position"
      :type_contract="type_contract"
      :editFilterCategoryList="editFilterCategoryList"
    />
    <ModalCreateEmployee
      v-if="modalCreateEmployee"
      :openCloseModal="openCloseModal"
      :editEmployeeList="editEmployeeList"
      :staffTag="staffTag"
      :position="position"
      :country="country"
      :type_contract="type_contract"
      :gender="gender"
    />
  </div>
</template>
