<template>
  <v-container>
    <v-card color="basil" flat class="justify-space-around">
        <v-container>
          <v-row>
            <v-col cols="12" md="8">
              <v-text-field v-model="text" label="text to send" required :counter=2048 />
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12" md="8">
              <v-text-field v-model="mentionedPerson" label="add mentioned person" required />
            </v-col>
            <v-col cols="12" md="4">
              <v-btn @click="addMentionedPerson" color="primary" label="Add" >Add</v-btn>
            </v-col>
          </v-row>
          <v-row>
            <v-chip-group  multiple v-model="all">
              <v-chip filter outlined>@all</v-chip>
            </v-chip-group>
            <v-chip :key="person" v-for="person in mentionedPersons" @click:close="remove(person)" class="ma-2" close color="green" outlined>
              {{ person }}
            </v-chip>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn color="primary" :disabled="text === '' || secretKey === ''" @click="sendText">Send</v-btn>
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
      name: "webhook_text",
      data: () => ({
          "text": '',
          "mentionedPerson": "",
          "mentionedPersons": [],
          "all": [],
          "snackbar": false,
      }),
      props: {
          sendUrl: String,
          secretKey: String
      },
      methods: {
          addMentionedPerson: function () {
            if ( ! this.mentionedPersons.includes(this.mentionedPerson))
              this.mentionedPersons.push(this.mentionedPerson)
          },
          remove: function (person) {
            this.mentionedPersons.splice(this.mentionedPersons.indexOf(person), 1)
            this.mentionedPersons = [...this.mentionedPersons]
          },
          sendText: function () {
            if (this.$props.secretKey === '') {
              this.snackbar = true
            } else {
              var mentioned_list = null
              if (this.all.length) {
                mentioned_list = this.mentionedPersons.concat(['@all'])
              } else {
                mentioned_list = this.mentionedPersons
              }
              const data = JSON.stringify({
                  'msgtype': 'text',
                  'text': {
                      'content': this.text,
                      'mentioned_list': mentioned_list
                  }
              })
              axios.post(this.$props.sendUrl, data, {
                params: {
                  key: this.$props.secretKey
                },
                headers: {
                    'Content-Type': 'text/plain'
                }
              })
            }
          }
      }
  }

</script>