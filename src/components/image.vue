<template>
  <v-container>
    <v-card color="basil" flat class="justify-space-around">
        <v-container>
          <v-row>
            <v-col cols="12" md="8">
              <v-file-input v-model="file" accept="image/*" label="Image input" @change="readImage"></v-file-input>
              <v-img v-if="dataURL !== ''" :src="dataURL" ></v-img>
           </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn color="primary" :disabled="this.secretKey === '' || this.rawData === ''" @click="sendText">Send</v-btn>
        </v-card-actions>
    </v-card>
    <v-snackbar v-model="snackbar" :timeout="3000" :top="true" :multi-line="true">
        {{ errorMsg }}
    </v-snackbar>
  </v-container>
</template>
<script>
import axios from 'axios'
import md5 from 'md5'

  export default {
      name: "webhook_image",
      data: () => ({
          "file": null,
          "dataURL": "",
          "rawData": "",
          "md5": "",
          "customImageMaxSize": 3, // megabytes
          "snackbar": false,
          "errorMsg": ""
      }),
      props: {
          sendUrl: String,
          secretKey: String
      },
      methods: {
        readImage () {
          var reader = new FileReader();
          reader.onload = (e) => {
             this.dataURL = e.target.result;
             this.rawData = this.dataURL.split('base64,')[1]
             this.md5 = md5(Buffer.from(this.rawData, 'base64'))
            }
          // read blob url from file data
          console.log(this.file)
          if (this.file !== null && this.file !== undefined) {
            if (this.file.size / 1024 / 1024 > 2) {
              this.errorMsg = "Maximum size of image is 2M"
              this.snackbar = true
            } else {
              reader.readAsDataURL(this.file);
            }
          } else {
            this.dataURL = ""
            this.rawData = ""
          }
        },
         sendText: function () {
            if (this.$props.secretKey === '') {
                this.errorMsg = "Please set key"
                this.snackbar = true
            } else {
              const data = JSON.stringify({
                  'msgtype': 'image',
                  'image': {
                      'base64': this.rawData,
                      'md5': this.md5
                  }
              })
              console.log(this.rawData)
              axios.post(this.$props.sendUrl, data, {
                params: {
                  key: this.$props.secretKey
                }
              })
            }
          }
      }
  }

</script>
