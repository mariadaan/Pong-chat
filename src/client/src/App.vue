<template>
  <Suspense>
    <Home />
  </Suspense>
  <CreateChannelModal />
</template>

<script setup>
import Home from './components/Home.vue';
import CreateChannelModal from './components/CreateChannelModal.vue';
import { provide } from '@vue/runtime-core';

import store from './store';
provide('store', store);

const { setUser, addMessage, currentChannel } = store();

// Get user from sessionStorage if exist
if (sessionStorage.getItem('user')) {
  const userData = JSON.parse(sessionStorage.getItem('user'));
  setUser(userData.id);
}

/**
 * Here we're checking the message sent from the server
 * through websocket. If the channelId from message and
 * our current channelId matches we are adding it to
 * our channelMessages state.
 */
connection.onmessage = function (message) {
  const data = JSON.parse(message.data);

  if (data.channelId === currentChannel.value.id) {
    addMessage(data);
  }
};
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=JetBrains+Mono');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --white1: #ffffff;
  --white2: #f9fbfc;
  --white3: #f0f0f0;
  --black1: #30333d;
  --blue1: #B2DAE7;
  --blue2: #c1e3ee;
  --blue3: #134279;
  --blue4: #def4fb;

  --blue1hover: linear-gradient(
    90deg,
    rgba(17, 134, 236, 0.43) 0%,
    rgba(19, 66, 121) 100%
    /* rgba(106, 0, 255, 0.42930675688244047) 0%, */
    );
    --gray1: #a4acb2;
    --gray2: #e9eff4;
}

.main {
  position: relative;
}

a {
  text-decoration: none;
  color: unset;
}

html,
body {
  height: 100%;
}

body {
  scroll-behavior: smooth;
  font-family: 'JetBrains Mono';
  font-style: normal;
  font-display: auto;
  overflow: hidden;
}
</style>
