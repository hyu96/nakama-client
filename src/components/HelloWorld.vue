<script setup>
import {Client} from "@heroiclabs/nakama-js";

(async() => {
  var useSSL = false; // Enable if server is run with an SSL certificate.
  var client = new Client("k!j2N*Gcvd#gAKy@mLCPpdutD#yMYIEW^(sKE$6wp7yE%C!eF@%fMbt8J6CESESg", "nakama-api-alb-1191308918.us-east-1.elb.amazonaws.com", "80", useSSL);
  const customId = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjc3NjgyODMwLCJleHAiOjE2ODAyNzQ4MzB9.uBRZB6Kw0brrnMbr9K-uSHkO69j-gS4Zzgz8Oz5tOXs";
  var session = await client.authenticateCustom(customId, false, "username");
  const socket = client.createSocket();
  var appearOnline = true;
  await socket.connect(session, appearOnline);
  var match = await socket.createMatch();
  console.log(match);

  const account = await client.getAccount(session);
  console.log(account)

  const roomName = "<match id>";
  const persistence = false;
  const hidden = false;
  // 1 = Room, 2 = Direct Message, 3 = Group
  const channel = await socket.joinChat(roomName, 1, persistence, hidden);
  console.log(channel)

  var channelId = channel.id;
  var data = { "message": "I think Red is the imposter!" };
  const messageAck = await socket.writeChatMessage(channelId, data);
  console.log(messageAck)
  var emoteData = {
      "emote": "point",
      "emoteTarget": "<redPlayerUserId>"
  }
  const emoteMessageAck = await socket.writeChatMessage(channelId, emoteData);
  console.log(emoteMessageAck)
})()

defineProps({
  msg: {
    type: String,
    required: true
  }
})


</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
