<template>
  <div class="email-display">
    <div>
      <button type="button" @click="toggleArchive">
        {{ email.read ? "Move to inbox (e)" : "Archive (e)" }}
      </button>
      <button type="button" @click="toggleRead">
        {{ email.read ? "Mark Unread (r)" : "Mark Read (r)" }}
      </button>
      <button type="button" @click="goNewer">Newer (k)</button>
      <button type="button" @click="goOlder">Older (j)</button>
    </div>
    <h2 class="mb-0">
      Subject: <strong>{{ email.subject }}</strong>
    </h2>
    <div>
      <em>From {{ email.from }} on {{ format(new Date(email.sentAt), "MMM do yyyy") }}</em>
    </div>
    <div v-html="marked(email.body)" />
  </div>
</template>

<script>
import { format } from "date-fns";
import marked from "marked";
import axios from "axios";
import useKeydown from "@/composables/use-keydown";
export default {
  setup({ email }) {
    let toggleRead = () => {
      email.read = !email.read;
      axios.put(`http://localhost:3000/emails/${email.id}`, email);
    };
    let toggleArchive = () => {
      email.archived = !email.archived;
      axios.put(`http://localhost:3000/emails/${email.id}`, email);
    };

    useKeydown([
      { key: "r", fn: toggleRead },
      { key: "e", fn: toggleArchive }
    ]);
    return {
      format,
      marked,
      toggleRead,
      toggleArchive
    };
  },
  props: {
    email: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped></style>
