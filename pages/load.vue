<template>
  <div class="" aria-labelledby="modal-title" role="dialog" aria-modal="true">
    <div class="flex items-start justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span class="hidden sm:inline-block sm:align-top sm:h-screen" aria-hidden="true">&#8203;</span>

      <div class="inline-block align-top bg-white rounded-lg px-4 pt-5 pb-4 text-left 
          overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-top sm:max-w-sm 
          sm:w-full sm:p-6">
      <div>

          <div class="">
              <form @submit.prevent="uploadNewFile" class="space-y-4">
                <div>
                  <p v-for="error, errorIdx in errors" :key="errorIdx" class="text-red-600 text-sm leading-tight 
                    text-center">
                    {{ error }}
                  </p>
                </div>

                  <!-- Files -->
                  <div>
                    <label class="lock text-sm font-medium text-gray-700">CSV File.</label>
                    <br>
                    <p class="lock text-xs text-gray-500 pb-2">
                      Upload your file
                    </p>
                    <VueFileAgent :deletable="true" :accept="'.csv'" 
                      :maxSize="'3MB'"
                      :maxFiles="1" :helpText="'Pick a file'"
                      :errorText="{ type: 'Invalid File', size: 'Files must be less than 3MB',}"
                      @select="imagesSelected($event)" @beforedelete="onBeforeDeleteImages($event)"
                      @delete="imageDeleted($event)" v-model="imageRecords" required>
                    </VueFileAgent>
                  </div>

                  <div>
                    <p class="lock text-sm font-medium text-gray-700">
                      Headers Configuration
                    </p>

                    <p class="lock text-xs text-gray-500">
                      Fill in each field based on the header of each column in your file
                    </p>
                  </div>

                  <!-- Name Header -->
                  <div>
                    <label for="name" class="block text-sm font-medium text-gray-700">Name</label>
                    <div class="mt-1">
                      <input v-model="name" id="name" name="name" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Address Header -->
                  <div>
                    <label for="address" class="block text-sm font-medium text-gray-700">Address</label>
                    <div class="mt-1">
                      <input v-model="address" id="address" name="address" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Birthday Header -->
                  <div>
                    <label for="birth_date" class="block text-sm font-medium text-gray-700">Birthday</label>
                    <div class="mt-1">
                      <input v-model="birth_date" id="birth_date" name="birth_date" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Credit Card Header -->
                  <div>
                    <label for="card" class="block text-sm font-medium text-gray-700">Credit Card</label>
                    <div class="mt-1">
                      <input v-model="card" id="card" name="card" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Telephone Header -->
                  <div>
                    <label for="phone" class="block text-sm font-medium text-gray-700">Telephone</label>
                    <div class="mt-1">
                      <input v-model="phone" id="phone" name="phone" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Email Header -->
                  <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
                    <div class="mt-1">
                      <input v-model="email" id="email" name="email" type="text" required 
                      class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md 
                        shadow-sm placeholder-gray-400 focus:outline-none focus:ring-green-400
                        focus:border-green-400 sm:text-sm">
                    </div>
                  </div>

                  <!-- Button -->
                  <div class="mt-5 sm:mt-6">
                      <button type="text" class="inline-flex justify-center w-full rounded-md border 
                      border-transparent shadow-sm px-4 py-2 bg-green-400 text-base font-medium text-white 
                      hover:bg-green-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 sm:text-sm">
                      Upload File
                      </button>
                  </div>
              </form>
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
      name:'',
      address:'',
      birth_date:'',
      card:'',
      email:'',
      phone:'',
      imageRecords: [],
      imageRecordsForUpload: [],
      errors:''
    }
  },
  methods:{
        deleteUploadedImageFile: function (imageRecord) {
            // Using the default uploader. You may use another uploader instead.
            this.$refs.vueFileAgent.deleteUpload(this.uploadUrl, this.uploadHeaders, imageRecord);
        },
        imagesSelected: function (imageRecordsNewlySelected) {
            var validImageRecords = imageRecordsNewlySelected.filter((fileRecord) => !fileRecord.error);
            this.imageRecordsForUpload = this.imageRecordsForUpload.concat(validImageRecords);
        },
        onBeforeDeleteImages: function (imageRecord) {
            var i = this.imageRecordsForUpload.indexOf(imageRecord);
            if (i !== -1) {
            // queued file, not yet uploaded. Just remove from the arrays
                this.imageRecordsForUpload.splice(i, 1);
                var k = this.imageRecords.indexOf(imageRecord);
                if (k !== -1) this.imageRecords.splice(k, 1);
            } else {
                if (confirm('Are you sure to remove the file?')) {
                this.$refs.vueFileAgent.deleteFileRecord(imageRecord); // will trigger 'delete' event
                }
            }
        },
        imageDeleted: function (imageRecord) {
            var i = this.imageRecordsForUpload.indexOf(imageRecord);
            if (i !== -1) {
                this.imageRecordsForUpload.splice(i, 1);
            } else {
                this.deleteUploadedImageFile(imageRecord);
            }
        },
        deleteUploadedFile: function (fileRecord) {
          // Using the default uploader. You may use another uploader instead.
            this.$refs.vueFileAgent.deleteUpload(this.uploadUrl, this.uploadHeaders, fileRecord);
        },
        filesSelected: function (fileRecordsNewlySelected) {
            var validFileRecords = fileRecordsNewlySelected.filter((fileRecord) => !fileRecord.error);
            this.fileRecordsForUpload = this.fileRecordsForUpload.concat(validFileRecords);
        },
        onBeforeDelete: function (fileRecord) {
            var i = this.fileRecordsForUpload.indexOf(fileRecord);
            if (i !== -1) {
            // queued file, not yet uploaded. Just remove from the arrays
                this.fileRecordsForUpload.splice(i, 1);
                var k = this.fileRecords.indexOf(fileRecord);
                if (k !== -1) this.fileRecords.splice(k, 1);
            } else {
                if (confirm('Are you sure to remove the file?')) {
                this.$refs.vueFileAgent.deleteFileRecord(fileRecord); // will trigger 'delete' event
                }
            }
        },
        fileDeleted: function (fileRecord) {
            var i = this.fileRecordsForUpload.indexOf(fileRecord);
            if (i !== -1) {
                this.fileRecordsForUpload.splice(i, 1);
            } else {
                this.deleteUploadedFile(fileRecord);
            }
        },
        uploadNewFile() {
          this.errors = ''
          const config = {
            'name': this.name,
            'address': this.address,
            'birth_date': this.birth_date,
            'card': this.card,
            'email': this.email,
            'phone': this.phone
          }
          let formData = new FormData();
          formData.append('headers_configuration', JSON.stringify(config) )
          console.log(this.imageRecords)
          if (this.imageRecords.length > 0){
            formData.append('file',  this.imageRecords[0].file)
          }
          
          this.$axios.post(`files/`, formData)
          .then((response) => {
            console.log(response)
            if(response.status == 201){
              this.$router.push('/contacts')
            }
          })
          .catch((error) => {
            this.errors = error.response.data
          })
        },
    }
}
</script>