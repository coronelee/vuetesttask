<script setup>
import EmployeeListComponent from './components/EmployeeListComponent.vue'
import CreateAndFilterEmployeeSheetComponent from './components/CreateAndFilterEmployeeSheetComponent.vue'
import ModalCreateEmployee from './components/ModalCreateEmployee.vue'
import EditEmployeeComponent from './components/EditEmployeeComponent.vue'
import { ref } from 'vue'

const staffTag = [
  {
    id: 1,
    bgColor: '#B0BCC7',
    rgbBg: 'rgb(176, 188, 199)',
    textColor: '#FFFFFF',
    title: 'Весь список'
  },
  {
    id: 2,
    bgColor: '#FFFDF1',
    rgbBg: 'rgb(255, 253, 241)',
    textColor: '#E2BD06',
    title: 'Проблемные',
    description: 'Истекает патент'
  },
  {
    id: 3,
    bgColor: '#FFF8F8',
    rgbBg: 'rgb(255, 248, 248)',
    textColor: '#E52E2E',
    title: 'Критические',
    description: 'Истекают все документы'
  },
  {
    id: 4,
    bgColor: '#F3FCFF',
    rgbBg: 'rgb(243, 252, 255)',
    textColor: '#00B6ED',
    title: 'Есть замечания',
    description: 'Пропустил медосмотр'
  },
  {
    id: 5,
    bgColor: '#F2FFF9',
    rgbBg: 'rgb(242, 255, 249)',
    textColor: '#00AE5B',
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
    type_contract: type_contract[2].title,
    type_contract_id: type_contract[2].id,
    gender: gender[0].title,
    gender_id: gender[0].id,
    country: country[0].title,
    country_id: country[0].id,
    position: position[0].title,
    position_id: position[0].id,
    status: staffTag[2].description,
    tag_id: staffTag[2].id + '',
    tag: 'Критические'
  }
])

const editModal = ref(false)
const valueEdit = ref([])
const openCloseEditModal = (value) => {
  valueEdit.value = value
  editModal.value = !editModal.value
}

const filterComponent = ref([
  {
    country_id: '',
    gender: '',
    position: '',
    type_contractTD: false,
    type_contractGPH: false,
    type_contractSMZ: false,
    type_contractKD: false
  }
])
const editEmployeeItem = (value, employee) => {
  for (let i = 0; i < employeeList.value.length; i++) {
    if (employeeList.value[i].full_name == value) {
      employeeList.value[i].full_name = employee[0]
      employeeList.value[i].inn = employee[1]
      employeeList.value[i].address = employee[2]
      employeeList.value[i].date_birth = employee[3]
      employeeList.value[i].age = employee[4]
      employeeList.value[i].type_contract = employee[5]
      employeeList.value[i].type_contract_id = employee[6]
      employeeList.value[i].gender = employee[7]
      employeeList.value[i].gender_id = employee[8]
      employeeList.value[i].country = employee[9]
      employeeList.value[i].country_id = employee[10]
      employeeList.value[i].position = employee[11]
      employeeList.value[i].position_id = employee[12]
      employeeList.value[i].status = employee[13]
      employeeList.value[i].tag_id = employee[14]
      employeeList.value[i].tag = employee[15]
    }
  }
}

const editFilterComponent = (value) => {
  filterComponent.value = value
}
const editEmployeeList = (value) => {
  employeeList.value.unshift(value)
}
const modalCreateEmployee = ref(false)

const openCloseModal = () => {
  modalCreateEmployee.value = !modalCreateEmployee.value
}
const filteredSearch = ref('')
const editFilteredSearch = (value) => {
  filteredSearch.value = value
}

const filterCategoryList = ref([])

const editFilterCategoryList = (value) => {
  if (value == 1) {
    filterCategoryList.value.length = 0
    for (let i = 1; i <= staffTag.length; i++) {
      document.getElementById(i).style.backgroundColor = staffTag[i - 1].bgColor
      document.getElementById(i).style.color = staffTag[i - 1].textColor
    }
  } else {
    if (filterCategoryList.value.includes(value)) {
      filterCategoryList.value.splice(filterCategoryList.value.indexOf(value), 1)
    } else {
      filterCategoryList.value.push(value)
    }
  }
}
</script>

<template>
  <div class="w-screen p-12 flex justify-between items-start gap-12 font-Montserrat">
    <EmployeeListComponent
      :employeeList="employeeList"
      :staffTag="staffTag"
      :position="position"
      :country="country"
      :type_contract="type_contract"
      :gender="gender"
      :editFilterCategoryList="editFilterCategoryList"
      :filterCategoryList="filterCategoryList"
      :editFilteredSearch="editFilteredSearch"
      :filteredSearch="filteredSearch"
      :filterComponent="filterComponent"
      :openCloseEditModal="openCloseEditModal"
    />
    <CreateAndFilterEmployeeSheetComponent
      :openCloseModal="openCloseModal"
      :country="country"
      :gender="gender"
      :position="position"
      :type_contract="type_contract"
      :editFilterCategoryList="editFilterCategoryList"
      :editFilterComponent="editFilterComponent"
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
    <EditEmployeeComponent
      v-if="editModal"
      :valueEdit="valueEdit"
      :openCloseEditModal="openCloseEditModal"
      :staffTag="staffTag"
      :position="position"
      :country="country"
      :type_contract="type_contract"
      :gender="gender"
      :editEmployeeItem="editEmployeeItem"
    />
  </div>
</template>
