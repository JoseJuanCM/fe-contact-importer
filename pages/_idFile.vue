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
                      Record
                    </th>
                    <th scope="col" class="px-6 py-3 text-left text-sm font-bold text-gray-500 tracking-wider">
                      Errors
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(error, index) in errorsArray" :key="index"
                    :class="index % 2 !== 0 ? 'bg-white' : 'bg-gray-50'">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{error.record}}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-gray-500">
                      {{error.error}}
                    </td>
                  </tr>
                </tbody>
              </table>
          </div>
          <div class="flex-1 flex justify-between sm:justify-end">
            <button v-if="previous" @click="previousPage" class="relative inline-flex items-center 
              px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 
              bg-white hover:bg-gray-50">
              Previous
            </button>
            <button v-if="next" @click="nextPage" class="ml-3 relative inline-flex items-center 
              px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 
              bg-white hover:bg-gray-50">
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
      fileId:'',
      errorsArray:'',
      previous:'',
      next:''
    }
  },
  mounted(){
    this.fileId = this.$route.params.idFile
    this.getFileErrors()
  },
  methods:{
    getFileErrors(){
      this.$axios.get(`file-errors/?file=${this.fileId}`)
        .then((response) => {
          this.errorsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    },
    nextPage(){
      const page = this.next.split("?").pop()
      this.$axios.get(`file-errors/?${page}`)
        .then((response) => {
          this.errorsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    },
    previousPage(){
      const page = this.previous.split("?").pop()
      this.$axios.get(`file-errors/?${page}`)
        .then((response) => {
          this.errorsArray = response.data.results
          this.previous = response.data.previous
          this.next = response.data.next
        })
    },
    getDetails(idFile){
      this.$router.push(`${idFile}/`)
    }
  },
}
</script>