<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-btn icon>
          <v-icon>mdi-email</v-icon>
        </v-btn>
        <v-toolbar-title>QY Wechat Robot Messager</v-toolbar-title>

      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/wujian752/RobotMessager"
        target="_blank"
        text
      >
        <span class="mr-2">Open in Github</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <MessagePanel/>
    </v-content>
  </v-app>
</template>

<script>
import MessagePanel from './components/MessagePanel';

export default {
  name: 'App',

  components: {
    MessagePanel,
  },

  data: () => ({
    //
  }),
};
</script>
