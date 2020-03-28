<template>
  <v-container>
    <v-card color="basil" flat class="justify-space-around">
        <v-container>
          <v-row>
            <v-col cols="12" md="6">
              <v-text-field v-model="title" :counter="42" label="title" required />
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field v-model="desc" :counter="170" label="description" />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12" md="6">
              <v-text-field v-model="url" label="url" />
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field v-model="picurl" label="picUrl" />
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn color="primary" :disabled="this.secretKey === '' || this.title === '' || this.url === ''"  @click="sendText">Send</v-btn>
        </v-card-actions>
    </v-card>
    <v-snackbar v-model="snackbar" :timeout="3000" :top="true" :multi-line="true">
        Please set key
    </v-snackbar>
  </v-container>
</template>
<script>
import axios from 'axios'
  export default {
      name: "webhook_article",
      data: () => ({
          "title": '',
          "desc": '',
          "url": '',
          "picurl": '',
          "snackbar": false,
      }),
      props: {
          sendUrl: String,
          secretKey: String
      },
      methods: {
          sendText: function () {
            if (this.$props.secretKey === '') {
                this.snackbar = true
            } else {
                const data = JSON.stringify({
                    'msgtype': 'news',
                    'news': {
                      "articles": [
                        {
                          "title": this.title,
                          "description": this.desc,
                          "url": this.url,
                          "picurl": this.picurl
                        }
                      ]
                    }
                })
                axios.post(this.$props.sendUrl, data, {
                  params: {key: this.$props.secretKey}
                })
            }
          }
      }
  }

</script>