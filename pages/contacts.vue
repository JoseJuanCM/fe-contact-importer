<template>
  <div>
    <div class="flex flex-col">
      <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
            <div class=" overflow-hidden">
              <table class="min-w-full  ">
                <thead>
                  <tr>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Name
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Birthday
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Email
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Telephone
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Credit Cart Last 4 Digits
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Brand
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Address
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(contact, index) in contactsArray" :key="index"
                    :class="index % 2 !== 0 ? 'bg-white' : 'bg-gray-50'">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.name}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{ $moment(contact.birth_date).format('YYYY MMMM DD') }}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.email}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.phone}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.last4}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.franchise}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{contact.address}}
                    </td>
                  </tr>
                </tbody>
              </table>
          </div>
          <div class="flex-1 flex justify-between sm:justify-end">
            <button v-if="previous" @click="previousPage" class="relative inline-flex items-center px-4 py-2 border border-gray-300 
              text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
              Previous
            </button>
            <button v-if="next" @click="nextPage" class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 
              text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
              Next
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'base',
  data(){
    return{
      contactsArray:'',
      previous:'',
      next:''
    }
  },
  mounted(){
    this.getContacts()
  },
  methods:{
    getContacts(){
      this.$axios.get(`contacts/`)
        .then((response) => {
          this.contactsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    },
    nextPage(){
      const page = this.next.split("?").pop()
      this.$axios.get(`contacts/?${page}`)
        .then((response) => {
          this.contactsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    },
    previousPage(){
      const page = this.previous.split("?").pop()
      this.$axios.get(`contacts/?${page}`)
        .then((response) => {
          this.contactsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    }
  },
}
</script>