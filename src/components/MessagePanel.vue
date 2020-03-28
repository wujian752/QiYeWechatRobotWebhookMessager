<template>
  <v-container>
    <v-col cols="12">
      <v-card>
        <v-card-title class="headline">Set your key</v-card-title>
          <v-col xs="12" sm="8" md="6">
             <v-text-field
               v-model="secretKey"
               :append-icon="showKey ? 'mdi-eye' : 'mdi-eye-off'"
               :rules="[rules.required, rules.min]"
               :type="showKey ? 'text' : 'password'"
               name="input-10-1"
               label="key for webhook"
               hint="At least 8 characters"
               counter
               @click:append="showKey = !showKey">
             </v-text-field>
          </v-col>
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-card color="basil">
        <v-tabs v-model="tab" background-color="transparent" color="basil" grow>
          <v-tab v-for="item in items" :key="item">
            {{ item }}
          </v-tab>
        </v-tabs>

        <v-tabs-items v-model="tab">
          <v-tab-item>
            <webhook_text v-bind:sendUrl="sendUrl" v-bind:secretKey="secretKey" />
          </v-tab-item>
          <v-tab-item>
            <webhook_markdown v-bind:sendUrl="sendUrl" v-bind:secretKey="secretKey" />
          </v-tab-item>
          <v-tab-item>
            <webhook_image v-bind:sendUrl="sendUrl" v-bind:secretKey="secretKey" />
          </v-tab-item>
          <v-tab-item>
            <webhook_article v-bind:sendUrl="sendUrl" v-bind:secretKey="secretKey" />
          </v-tab-item>
        </v-tabs-items>
      </v-card>
    </v-col>
  </v-container>
</template>

<script>
import webhook_text from './text'
import webhook_markdown from './markdown'
import webhook_image from './image'
import webhook_article from './article'
  export default {
    name: 'MessagePanel',
    components: {
      webhook_text,
      webhook_markdown,
      webhook_image,
      webhook_article,
    },

    data: () => ({
      secretKey: '',
      showKey: false,
      rules: {
          required: value => !!value || 'Required.',
          min: v => v.length == 36 || '36 characters',
        },
      items: ['text', 'Markdown', 'Image', 'Article'],
      tab: 'text',
      sendUrl: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send",
    }),
  }
</script>
