<template>
  <aside class="sm-side">
    <div class="user-head">
      <img src="src/assets/images/profile.jpg">

      <div class="user-name">
        <h5>Veronica Burzynska</h5>
        <span class="email-address">info@codingexplained.com</span>
      </div>
    </div>
    <div class="compose-wrapper">
      <app-compose></app-compose>
    </div>
    <ul class="inbox-nav">
      <li :class="{active:  activeView === 'app-inbox'}">
        <a href="#" @click.prevent="navigate('app-inbox')">
          <i class="fa fa-inbox"></i>Inbox <span class="label label-danger pull-right">{{unreadMessages.length}}</span>
        </a>
      </li>

      <li :class="{active:  activeView === 'app-send'}">
        <a href="#" @click.prevent="navigate('app-send')">
          <i class="fa fa-envelope-o"></i>Sent <span class="label label-default pull-right">{{sentMessages.length}}</span>
        </a>
      </li>

      <li :class="{active:  activeView === 'app-important'}">
        <a href="#" @click.prevent="navigate('app-important')">
          <i class="fa fa-bookmark-o"></i>Important <span class="label label-warning pull-right">{{importantMessages.length}}</span>
        </a>
      </li>

      <li :class="{active:  activeView === 'app-trash'}">
        <a href="#" @click.prevent="navigate('app-trash')">
          <i class=" fa fa-trash-o"></i>Trash <span class="label label-default pull-right">{{trashMessages.length}}</span>
        </a>
      </li>
    </ul>
  </aside>
</template>

<script>
  import {eventBus} from './main'
  import Compose from './compose'
  export default {
    props: {
      messages: {
        type: Array,
        require: true
      }
    },
    name: "sidebar",
    data() {
      return {
        activeView: 'app-inbox'
      }
    },
    created() {
      eventBus.$on('changeView', (data) => {
        this.activeView = data.tag;
      });

    },
    methods: {
      navigate(e) {
        eventBus.$emit('changeView', {
          tag: e,
          title: e.split('-')[1]
        });
      }
    },
    computed: {
      unreadMessages() {
        return this.messages.filter(function (message) {
          return (message.type == 'incoming' && !message.isRead && !message.isDeleted);
        });
      },
      sentMessages() {
        return this.messages.filter(function (message) {
          return (message.type == 'outgoing' && !message.isDeleted);

        });
      },
      importantMessages() {
        return this.messages.filter(function (message) {
          return (message.type == 'incoming' && message.isImportant === true && !message.isDeleted);

        });
      },
      trashMessages() {
        return this.messages.filter(function (message) {
          return (message.isDeleted === true);
        });
      }
    },
    components: {
      appCompose: Compose
    }
  }
</script>

<style scoped>

</style>
