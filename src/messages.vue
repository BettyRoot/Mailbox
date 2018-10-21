<template>
  <table class="table table-inbox table-hover">
    <tbody><input type="text" v-model="searching">
    <tr @click="openMessage(message)" v-for="message in filteredMessage"
        :class="{ unread: typeof message.isRead !== 'undefined' && !message.isRead }">
      <td><input type="checkbox"></td>
      <td>
        <a href="#" v-if="typeof message.isImportant !== 'undefined'"
           @click.prevent.stop="message.isImportant = !message.isImportant">
          <i :class="['fa', 'fa-star', {important: message.isImportant}]"></i>
        </a>
      </td>
      <td>{{ message.from.name }}</td>
      <td>{{ message.subject }}</td>
      <td><i v-if="message.attachments.length > 0" class="fa fa-paperclip"></i></td>
      <td class="text-right">{{ message.date.fromNow() }}</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
  import {eventBus} from './main';

  export default {
    name: "messages",
    data() {
      return {
        searching: ''
      }
    },
    props: {
      messages: {
        type: Array,
        required: true
      }
    },
    methods: {
      openMessage(message) {
        eventBus.$emit('changeView', {
          tag: 'app-view-message',
          title: message.subject,
          data: {
            message: message
          }


        })
      }
    },
    computed:
      {
        filteredMessage() {
          return this.messages.filter((message) => {
            // return message.subject.toLowerCase().includes(this.searching.toLowerCase());
            return message.subject.toLowerCase().indexOf(this.searching.toLowerCase())!==-1;
          });

        }
      }
  }
</script>

<style scoped>

</style>
