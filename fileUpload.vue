<template>
    <div class="fileupload-container">
        <form enctype="multipart/form-data" name="vue-fileupload" novalidate v-if="isInitial || isSaving">
            <slot name="header">
                <i class="fd-icon-upload light-blue" onclick="$('input[id=fileUploadInput]').click();"></i>
            </slot>
            <div class="dropbox">
                <input type="file" multiple id="fileUploadInput" :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.files)"
                       accept="image/*" class="input-file" style="display:none">
                <p v-if="isInitial" onclick="$('input[id=fileUploadInput]').click();" class="light-blue">
                    Click here to upload
                </p>
                <p class="light-blue" v-if="isSaving">
                    Uploading file...
                </p>
            </div>

        </form>

        <div v-if="isSuccess">
            <p class="light-blue" v-if="displaySuccess">Success!</p>
            <p v-else>&nbsp;</p>
            <img v-for="uploadedFile in uploadedFiles" v-bind:src="uploadedFile.preview">
        </div>

        <div v-if="isFailed">
            <h2>Upload failed.</h2>
            <p>
                <a href="javascript:void(0)" @click="reset()">Try again</a>
            </p>
            <pre>{{ uploadError }}</pre>
        </div>
    </div>

</template>

<script>
    const STATUS_INITIAL = 0, STATUS_SAVING = 1, STATUS_SUCCESS = 2, STATUS_FAILED = 3;
    export default {
        props:    ['uploadData', 'url'],
        data() {
            return {
                uploadedFiles: [],
                uploadError: null,
                currentStatus: null,
                uploadFieldName: 'photos',
                displaySuccess: false
            }
        },
        computed: {
            isInitial() {
                return this.currentStatus === STATUS_INITIAL;
            },
            isSaving() {
                return this.currentStatus === STATUS_SAVING;
            },
            isSuccess() {
                return this.currentStatus === STATUS_SUCCESS;
            },
            isFailed() {
                return this.currentStatus === STATUS_FAILED;
            }
        },
        methods: {
            reset() {
                this.currentStatus = STATUS_INITIAL;
                this.uploadedFiles = [];
                this.uploadError = null;
            },
            successTimer: function(){
                let self = this;
                setTimeout(function() {
                    self.displaySuccess = false;
                }, 3000);
            },
            save(formData) {
                // upload data to the server
                let self = this;
                this.currentStatus = STATUS_SAVING;
                this.$http.post(this.url, formData).then( function (response) {
                    self.uploadedFiles = [].concat(response.data);
                    self.$emit('uploaded', response.data);
                    self.currentStatus = STATUS_SUCCESS;
                    self.displaySuccess = true;
                    self.successTimer();
                }, function (error) {
                    self.uploadError = error.response;
                    self.currentStatus = STATUS_FAILED;
                });

            },
            filesChange(fileList) {
                const formData = new FormData();
                let self = this;

                if (!fileList.length) return;

                Object.keys(fileList).map(function(x){
                    formData.append('file', fileList[x]);
                });
                Object.keys(this.uploadData).map(function(x){
                   formData.append(x, self.uploadData[x]);
                });

                this.save(formData);
            }
        },
        mounted() {
            this.reset();
        },
    }
</script>
