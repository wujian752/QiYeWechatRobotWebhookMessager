<template>
  <v-container>
    <v-card color="basil" flat class="justify-space-around">
        <v-container>
          <v-row>
            <v-col cols="12" md="8">
              <v-textarea
                v-model="text"
                solo
                name="input-7-4"
                label="Markdown Content"
              ></v-textarea>
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn color="primary" :disabled="this.secretKey === '' || this.text === ''"  @click="sendText">Send</v-btn>
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
      name: "webhook_markdown",
      data: () => ({
          "text": '',
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
                    'msgtype': 'markdown',
                    'markdown': {
                        'content': this.text,
                    }
                })
                axios.post(this.$props.sendUrl, data, {
                  params: {key: this.$props.secretKey},
                })
            }
          }
      }
  }

</script>