<script setup>

import { ref, computed } from 'vue';

const showHiddenDiv = ref(false);
const employees = ref([]);
const newEmployee = ref({
    photoURL:'',
    firstName: '',
    middleName: '',
    lastName: '',
    email: '',
    department: '',
    idNo: '',
    active: false,
    deactivated: false,
});

const isResetModalVisible = ref(false);
function openResetModal() {
    isResetModalVisible.value = true;
}

function closeResetModal() {
    isResetModalVisible.value = false;
}



// Reset button
function resetEmployee() {
  if (selectedEmployeeIndex.value !== null) {
    const originalEmployee = employees.value[selectedEmployeeIndex.value];
    Object.assign(originalEmployee, newEmployee.value, {
      date: formattedDate,
    });
    //selectedEmployeeIndex.value = null;
    isResetModalVisible.value = false;
  }
}

function resetForm() {
  newEmployee.value = {
    photoURL: '',
    firstName: '',
    middleName: '',
    lastName: '',
    email: '',
    department: '',
    idNo: '',
    active: false,
    deactivated: false,
  };
}


const isDeleteModalVisible = ref(false);

function openDeleteModal() {
  isDeleteModalVisible.value = true;
}
function closeDeleteModal() {
  isDeleteModalVisible.value = false;
}

function toggleHiddenDiv() {
  showHiddenDiv.value = !showHiddenDiv.value;
}

function handleImageUpload(event) {
    const image = event.target.files[0];
    newEmployee.value.photoURL = URL.createObjectURL(image);
}

function addEmployee(){
    employees.value.push({
        ...newEmployee.value,
        date: formattedDate,
    });

  newEmployee.value = {
    photoURL:'',
    firstName: '',
    middleName: '',
    lastName: '',
    email: '',
    department: '',
    idNo: '',
    active: false,
    deactivated: false,
  };
}

const formattedDate = new Date().toLocaleDateString(undefined, {
  year: 'numeric',
  month: 'short',
  day: 'numeric'
});

const selectedEmployeeIndex = ref(null);

function showDetails(index) {
  selectedEmployeeIndex.value = index;
}

const isEditMode = ref(false);

//Validation
const isFormInvalid = computed(() => {
  return (
    !newEmployee.value.firstName ||
    !newEmployee.value.lastName ||
    !newEmployee.value.email ||
    !newEmployee.value.department ||
    !newEmployee.value.idNo
  );
});

// Delete Employee
function deleteRow() {
    if (selectedEmployeeIndex.value !== null) {
        employees.value.splice(selectedEmployeeIndex.value, 1);
        selectedEmployeeIndex.value = null;
        isDeleteModalVisible.value = false;
    }
  }

function hideDetails() {
  selectedEmployeeIndex.value = null;
}

const selectedEmployee = computed(() => {
  if (selectedEmployeeIndex.value !== null) {
    return employees.value[selectedEmployeeIndex.value];
  }
  return null;
});

</script>

<template>
    <div class="flex space-x-4 mx-4 pt-4">

        <!-- Search by Name-->
        <div class="relative w-96 max-w-xs ml-4">
            <form>
                <div class="flex justify-between overflow-hidden rounded-md bg-white border border-gray-300">
                    <input type="text" name="search" placeholder="Search by name" autocomplete="off" aria-label="Search talk" class="block w-full flex-1 px-3 focus:outline-none">
                    <span class="m-1 inline-flex cursor-pointer items-center rounded-md bg-white-600 p-2 hover:bg-white-700">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="currentColor" d="m19.6 21l-6.3-6.3q-.75.6-1.725.95T9.5 16q-2.725 0-4.612-1.888T3 9.5q0-2.725 1.888-4.612T9.5 3q2.725 0 4.612 1.888T16 9.5q0 1.1-.35 2.075T14.7 13.3l6.3 6.3l-1.4 1.4ZM9.5 14q1.875 0 3.188-1.313T14 9.5q0-1.875-1.313-3.188T9.5 5Q7.625 5 6.312 6.313T5 9.5q0 1.875 1.313 3.188T9.5 14Z"/></svg>
                    </span>
                </div>
            </form>
        </div>

        <!-- Filter Icon-->
        <div class="relative w-96 max-w-xs ml-4">
            <form>
                <div class="flex justify-between overflow-hidden rounded-md bg-white border border-gray-300">
                    <span class="m-1 inline-flex cursor-pointer items-center rounded-md bg-red-600 p-2 hover:bg-red-700">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="white" fill-rule="evenodd" d="M4.953 2.25h14.094c.667 0 1.237 0 1.693.057c.483.061.95.198 1.334.558c.39.367.545.824.613 1.299c.063.437.063.98.063 1.6v.776c0 .489 0 .91-.036 1.263c-.04.38-.125.735-.331 1.076c-.205.339-.48.585-.798.805c-.299.208-.68.423-1.13.676l-2.942 1.656c-.67.377-.903.513-1.059.648c-.357.31-.562.655-.658 1.086c-.041.185-.046.417-.046 1.123v2.732c0 .901 0 1.666-.093 2.255c-.098.625-.327 1.225-.927 1.6c-.587.367-1.232.333-1.86.184c-.605-.143-1.35-.435-2.244-.784l-.086-.034c-.42-.164-.786-.307-1.076-.457c-.312-.161-.602-.361-.823-.673c-.225-.316-.314-.654-.355-1c-.036-.315-.036-.693-.036-1.115v-2.708c0-.706-.004-.938-.046-1.123a1.933 1.933 0 0 0-.658-1.086c-.156-.135-.39-.271-1.059-.648L3.545 10.36c-.45-.253-.831-.468-1.13-.676c-.318-.22-.593-.466-.798-.805c-.206-.341-.291-.697-.33-1.076c-.037-.352-.037-.774-.037-1.263v-.776c0-.62 0-1.163.063-1.6c.068-.475.223-.932.613-1.299c.384-.36.85-.497 1.334-.558c.456-.057 1.026-.057 1.693-.057ZM3.448 3.796c-.334.042-.44.11-.495.163c-.05.046-.114.127-.156.418c-.045.318-.047.752-.047 1.438v.69c0 .534 0 .878.028 1.144c.026.247.07.366.124.455c.055.091.147.194.368.348c.234.162.553.343 1.04.617l2.913 1.64l.08.045c.56.315.94.529 1.226.777a3.43 3.43 0 0 1 1.14 1.893c.081.367.081.78.081 1.36v2.759c0 .472.001.762.027.98c.022.198.059.265.086.304c.03.042.09.107.289.21c.212.109.505.224.967.405c.961.376 1.608.627 2.097.743c.479.114.637.055.718.004c.068-.043.173-.13.242-.563c.072-.457.074-1.103.074-2.084v-2.758c0-.58 0-.993.082-1.36a3.43 3.43 0 0 1 1.139-1.893c.286-.248.667-.463 1.225-.777l.081-.045l2.913-1.64c.487-.274.806-.455 1.04-.617c.221-.154.313-.257.368-.348c.054-.089.098-.208.123-.455c.028-.266.029-.61.029-1.145v-.69c0-.685-.002-1.12-.047-1.437c-.042-.291-.107-.372-.155-.418c-.056-.052-.162-.121-.496-.163c-.35-.045-.825-.046-1.552-.046H5c-.727 0-1.201.001-1.552.046Z" clip-rule="evenodd"/></svg>
                    </span>
                    <input type="text" name="search" placeholder="department..." autocomplete="off" aria-label="Search talk" class="block w-full flex-1 px-3 py-2 focus:outline-none">
                </div>
            </form>
        </div>

        <!-- New Button -->
        <div class="flex-1 mx-4"></div> 
        <button class="bg-red-600 px-3 py-1 rounded text-white" @click="toggleHiddenDiv"> + New </button>
        </div>

    <!-- Hidden Div -->
    <div v-if="showHiddenDiv" class="my-10 border-t-[1px] border-[#D3D3D3] w-[100%]">
      <div class="flex justify-evenly mt-10 w-100 gap-[10em] px-[5em]">
        <div class="flex flex-col justify-center items-center">
          <label for="image-upload" class="broken-square cursor-pointer relative w-50 h-50 ml-1">
                <img v-if="newEmployee.photoURL" :src="newEmployee.photoURL" alt="Uploaded" class="w-full h-full object-cover" />
                <div v-else class="border-dashed absolute inset-0 rounded flex items-center justify-center">
                <img src="~/assets/Upload.png" alt="Upload" class="w-50 h-50 ml-1" />
                </div>
                <input type="file" id="image-upload" class="hidden" @change="handleImageUpload" />
            </label>
            <div>
                <label class="block text-[#667085] mb-[2em] text-sm mt-2" for="uploadPhoto">{{ uploadedPhotoSize }}</label>
                <div class="text-center  font-normal text-gray-500">720 KB</div>
                
                <input id="image-upload" type="file" class="hidden">
        <label for="image-upload" class="block mt-1 text-sm text-[#00A2FD] cursor-pointer justify-center" @change="handleImageUpload">Upload Image</label>
            </div>
          </div>

          <div class="grid grid-cols-3 gap-4 w-[100%]">
                
                <!-- First Name -->
                <div>
                    <label for="first-name" class="block font-normal mb-2">First Name <span class="text-[#E42323]">*</span></label>
                    <input v-model="newEmployee.firstName" type="text" id="first-name" class="w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500" />
                </div>

                <!-- Middle Name -->
                <div>
                    <label for="middle-name" class="block font-normal mb-2">Middle Name</label>
                    <input v-model="newEmployee.middleName" type="text" id="middle-name" class="w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500" />
                </div>

                <!-- Last Name -->
                <div>
                    <label for="last-name" class="block font-normal mb-2">Last Name <span class="text-[#E42323]">*</span></label>
                    <input v-model="newEmployee.lastName" type="text" id="last-name" class="w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500" />
                </div>

                <!-- Email -->
                <div>
                    <label for = "email" class = "block font-normal mb-2"> Email <span class ="text-[#E42323]">*</span></label>
                    <input v-model="newEmployee.email" type = "text" id = "email" class = "w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500"/>
                </div>

                <!-- Department -->
                <div>
                    <label for = "department" class = "block font-normal mb-2"> Department <span class ="text-[#E42323]">*</span></label>
                    <input v-model="newEmployee.department" type = "text" id = "department" class = "w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500"/>
                </div>

                <!-- ID No. -->
                <div>
                    <label for = "id-no" class = "block font-normal mb-2"> ID No. <span class ="text-[#E42323]">*</span></label>
                    <input v-model="newEmployee.idNo" type = "text" id = "id-no" class = "w-full border border-gray-300 px-3 py-2 rounded focus:outline-none focus:border-indigo-500"/>
                </div>

                <div class="flex items-center space-x-5">
                  <div class="flex items-center space-x-1">
                    <input v-model="newEmployee.active" type="checkbox" id="active-checkbox" class = "mr-2" />
                        <label for="active-checkbox" class="font-normal">Active</label>
                                
                        <input v-model="newEmployee.deactivated" type="checkbox" id="deactivated-checkbox" class = "ml-4 mr-2" />
                        <label for="deactivated-checkbox" class="font-normal">Deactived</label>
                  </div>
                </div>
          </div>

          <div class="grid">
            <button class="relative w-36 max-w-xs px-10 py-1 h-[60px] items-center bg-[#2E8F00] text-white font-normal" type="button" :disabled="isFormInvalid"
@click="addEmployee">Add</button>            
            <button class="relative w-36 max-w-xs px-10 py-1 h-[60px] items-center bg-[#D9D9D9] text-white font-normal" type="button" @click="resetForm">Cancel</button>
          </div>
      </div>
    </div>

    <div class="w-[100%] flex flex-row-reverse">

  <!-- Employees Details -->
  <div v-if="selectedEmployeeIndex !== null" class="my-10 pl-5 pr-5 border-1 bg-[#F8F7F7] rounded-none w-[25%]">
    <div class="flex gap-2 flex-row-reverse p-[1rem]">
      <div class="inline-flex cursor-pointer items-center bg-[#E42326] rounded-md p-2.5" @click="openDeleteModal">
        <svg xmlns="
http://www.w3.org/2000/svg
" width="24" height="24" viewBox="0 0 24 24"><path fill="white" d="M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6v12M8 9h8v10H8V9m7.5-5l-1-1h-5l-1 1H5v2h14V4h-3.5Z"/></svg>
      </div>
      <!--Delete Button-->
      <!-- Delete Modal -->
      <div v-if="isDeleteModalVisible" class="modal fixed inset-0 flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="bg-white rounded-md border-t-[1rem] border-[#E42323] p-10 mx-2 grid gap-4 h-[50%] w-[35%]">
                <div class="content flex flex-col items-center justify-center text-center p-5">
                  <div class="flex items-center justify-center">
                      <img class="h-[80px] w-[80px] mt-[-50px]" src="~/assets/Trash bin.png" alt="Reset Icon">
                      <div class="ml-6 mx-2">
                          <div class="text-lg font-bold text-left">Delete Profile</div>
                          <div class="mt-2 font-medium font-[12px] text-left justify-center">You'll permanently delete all the data, messages, photos and important information.</div>
                      </div>
                  </div>
                  <div class="flex space-x-4 mt-14">
                      <button class="relative w-36 max-w-xs px-10 py-1 h-[60px] items-center bg-[#D9D9D9] text-white font-[12px] font-medium rounded-md" @click="closeDeleteModal" type="button">Cancel</button>
                      <button @click="deleteRow" class="px-11 py-2 bg-[#E42323] text-white rounded-md">Delete</button>                  
                  </div>
                </div>
        </div>
      </div>


      <div class="inline-flex cursor-pointer items-center bg-[#2E8F00] rounded-md p-2.5">
        <span class="m-1 inline-flex cursor-pointer items-center rounded-md" @click="isEditMode = !isEditMode">
          <svg xmlns="http://www.w3.org/2000/svg"  width="24" height="24" viewBox="0 0 24 24"><g fill="white"><path fill-rule="evenodd" d="M21.264 2.293a1 1 0 0 0-1.415 0l-.872.872a3.001 3.001 0 0 0-3.415.587L4.955 14.358l5.657 5.657L21.22 9.408a2.999 2.999 0 0 0 .586-3.414l.873-.873a1 1 0 0 0 0-1.414l-1.415-1.414Zm-4.268 8.51l-6.384 6.384l-2.828-2.829l6.383-6.383l2.829 2.829Zm1.818-1.818l.99-.99a1 1 0 0 0 0-1.415l-1.413-1.414a1 1 0 0 0-1.415 0l-.99.99l2.828 2.83Z" clip-rule="evenodd"/><path d="m2 22.95l2.122-7.778l5.656 5.657L2 22.95Z"/></g></svg>
        </span>
      </div>

      <div class="inline-flex cursor-pointer items-center bg-[#FFC700] rounded-md p-2.5" @click="openResetModal">
        <svg xmlns="
http://www.w3.org/2000/svg
" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="white" stroke-width="2" d="M20 8c-1.403-2.96-4.463-5-8-5a9 9 0 1 0 0 18a9 9 0 0 0 9-9m0-9v6h-6"/></svg>
      </div>
      <!-- Reset Button-->
      <div v-if="isResetModalVisible" class="modal fixed inset-0 flex justify-center items-center z-50 bg-black bg-opacity-50">
        <div class="bg-white rounded-md border-t-[1rem] border-[#F4B000] p-10 mx-2 grid gap-4 h-[50%] w-[35%]">
                <div class="content flex flex-col items-center justify-center text-center p-5">
                  <div class="flex items-center justify-center">
                      <img class="h-[80px] w-[80px] mt-[-50px]" src="~/assets/reboot.png" alt="Reset Icon">
                      <div class="ml-6 mx-2">
                          <div class="text-lg font-bold text-left">Reset Profile</div>
                          <div class="mt-2 font-medium font-[12px] text-left justify-center">Are you sure you want to reset this profile? If you reset this profile, you will permanently lose all the important data.</div>
                      </div>
                  </div>
                  <div class="flex space-x-4 mt-14">
                      <button class="relative w-36 max-w-xs px-10 py-1 h-[60px] items-center bg-[#D9D9D9] text-white font-[12px] font-medium rounded-md" @click="closeResetModal" type="button">Cancel</button>
                      <button
            class="relative w-36 max-w-xs px-10 py-1 h-[60px] items-center bg-[#F4B000] text-white font-[12px] font-medium rounded-md"
            @click="resetEmployee"
            type="button"
          >
            Reset
          </button>
                  </div>
                </div>
        </div>
      </div>

    </div>

    <div class="flex items-start mb-4">
      <!-- Display selected employee's image -->
      <div class="w-20 h-20 rounded overflow-hidden">
        <img
          v-if="selectedEmployee.photoURL"
          :src="selectedEmployee.photoURL"
          alt="Employee Photo"
          class="w-full h-full object-cover"
        />
        <div v-else class="border-dashed border-gray-300 h-full flex items-center justify-center">
          <span class="text-gray-400">No photo</span>
        </div>
      </div>
      
      <!-- Upload new image -->
      <div class="w-1/2 pl-2 pr-4 p-5 text-center h-[75%] w-[50%]">
        <input id="image-upload" type="file" class="hidden">
        <label for="image-upload" class="block mt-1 text-sm text-[#00A2FD] cursor-pointer text-left" @change="handleImageUpload">Upload Image</label>
        <p class="block text-left text-[#766E6E]">
          {{ selectedEmployeeIndex !== null ? employees[selectedEmployeeIndex].firstName + ' ' + employees[selectedEmployeeIndex].middleName + ' ' + employees[selectedEmployeeIndex].lastName : 'Upload Image' }}
        </p>
        <p class="block text-left text-[#766E6E]">
          {{ selectedEmployeeIndex !== null ? employees[selectedEmployeeIndex].idNo : '' }}
        </p>
      </div>    
      
  </div>

    <div class="col-span-2">
    <div class="grid grid-rows-3 grid-flow-col gap-[25px] mt-2">
      <!-- First Name -->
      <div>
          <label for="first-name" class="block font-normal mb-2">First Name</label>
          <input type="text" v-model="selectedEmployee.firstName" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      

      </div>
  
      <!-- Middle Name -->
      <div>
          <label for="middle-name" class="block font-normal mb-2">Middle Name</label>
          <input type="text" v-model="selectedEmployee.middleName" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      
      </div>
  
      <!-- Last Name -->
      <div>
          <label for="last-name" class="block font-normal mb-2">Last Name</label>
          <input type="text" v-model="selectedEmployee.lastName" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      
      </div>
      
      <!-- Email-->
      <div>
          <label for="email" class="block font-normal mb-2"> Email</label>
          <input type="email" v-model="selectedEmployee.email" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      
      </div>
  
      <!-- Department-->
      <div>
          <label for="department" class="block font-normal mb-2"> Department</label>
          <input type="text" v-model="selectedEmployee.department" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      
      </div>
  
      <!-- ID No.-->
      <div>
          <label for="id-no" class="block font-normal mb-2"> ID No.</label>
          <input type="text" v-model="selectedEmployee.idNo" :disabled="!isEditMode" class="border rounded p-1 w-full" disabled>      
      </div>
  </div>
  <div class="flex mt-2 items-center">
  
    <!-- Checkboxes -->
    
<div>
  <input v-model="selectedEmployee.active" type="checkbox" id="active-checkbox" :disabled="!isEditMode" class="mr-1" disabled :class="{'text-blue-500': selectedEmployee.active, 'text-red-500': !selectedEmployee.active}" />
  <label for="active-checkbox" class="font-medium">Active</label>
</div>

<div class="ml-1 mt-[-2]"> <!-- Added "mt-2" for top margin -->
  <input v-model="selectedEmployee.deactivated" type="checkbox" id="deactivated-checkbox" :disabled="!isEditMode" class="mr-1" disabled :class="{'text-blue-500': !selectedEmployee.deactivated, 'text-red-500': selectedEmployee.deactivated}" />
  <label for="deactivated-checkbox" class="font-medium text-red-500">Deactived</label>
</div>
</div>
</div>
    

    <div class="flex justify-end mt-2 space-x-2 pt-6 p-[1rem]">
    <button class="px-4 py-2 bg-[#D9D9D9] text-white rounded" @click="hideDetails">Cancel</button>
    <button class="px-4 py-2 bg-[#308F00] text-white rounded" @click="updateEmployee(selectedEmployeeIndex)">Update</button>
  </div>
  </div>



    <div class = "mt-4 mx-4 w-full" :class="{'!w-[75%]': selectedEmployeeIndex !== null}" >
        <h2 class = "pt-5 pl-4 pb-5 text-lg font-semibold text-black-800 flex items-center">
            Employees Account
            <span class = "flex-grow h-1 bg-red-300 mx-4"></span>
        </h2>
                <!-- Static Table for Employee Accounts -->
        <div class="px-4 mt-1">
            <div class="bg-white rounded mx-4">
            <table class="min-w-full">
                <thead>
                <tr>
                    <th class="py-2 px-1 font-light text-gray-500 text-left flex items-center">
                    <label class="inline-flex items-center">
                        <input type="checkbox" class="form-checkbox h-4 w-4 text-indigo-600">
                        <span class="ml-2">File Name/ID No.</span>
                    </label>
                    </th>
                    
                    <th class="py-2 px-3 font-light text-gray-500 text-left">Date</th>
                    <th class="py-2 px-3 font-light text-gray-500 text-left">Email</th>
                    <th class="py-2 px-3 font-light text-gray-500 text-left">Department</th>
                    <th class="py-2 px-3 font-light text-gray-500 text-left">Active</th>
                </tr>
                </thead>

                <tbody>
                <tr v-for="(employee, index) in employees" :key="index" @click="showDetails(index)" :class="index % 2 === 0 ? 'even-row' : 'odd-row'">
                    <td class="py-2 px-1">
                    <div class="flex items-center space-x-2">
                      <label class="inline-flex items-center mr-2">
                          <input type="checkbox" class="form-checkbox h-4 w-4 text-indigo-600">
                      </label>
                        <div class="w-10 h-10 rounded-none overflow-hidden">
                        <img
                            v-if="employee.photoURL"
                            :src="employee.photoURL"
                            alt="Employee Photo"
                            class="w-full h-full object-cover"
                        />
                        </div>
                        <div class="text-[#101828]">
                        <div>{{ employee.firstName }} {{ employee.middleName }} {{ employee.lastName }}</div>
                        <div class="text-[#667085]">{{ employee.idNo }}</div>
                        </div>
                    </div>
                    </td>
                    <td class="text-[#667085] py-2 px-3">{{ formattedDate }}</td>
                    <td class="text-[#667085] py-2 px-3">{{ employee.email }}</td>
                    <td class="text-[#667085] py-2 px-3">{{ employee.department }}</td>
                    <td class="text-[#667085] py-2 px-3 font-semibold" :style="{'color': employee.active ? 'green' : (employee.deactivated ? 'red' : '')}">
                    {{ employee.active ? 'Active' : (employee.deactivated ? 'Deactivated' : '')}}
                    </td>
                    <!-- Dropdown Menu Column --> 
                    <td>
                      <div>
                        <img src="~/assets/dropdown.png">
                      </div>
                    </td>
                </tr>
                </tbody>
            </table>
            </div>
        </div>
    </div>
    </div>

    
   
</template>
<style scoped>
.broken-square {
  display: inline-block;
  position: relative;
  width: 150px;
  height: 150px;
  border: 2px dashed #D3D3D3;
}

.even-row {
    background-color: #ffffff; /* Default color for odd rows */
  }

  .odd-row {
    background-color: #EAECF0; /* Lighter color for even rows */
  }

</style>





