<template>
  <aside class="lg-side">
    <div class="inbox-head">
      <h3>{{currentView.title}}</h3>
    </div>
    <keep-alive>
      <component :is="currentView.tag" :data="currentView.data"></component>
    </keep-alive>
  </aside>

</template>

<script>
  import {eventBus} from './main'
  import Inbox from './inbox'
  import Send from './send'
  import Trash from './trash'
  import Important from './important'
  import ViewMessage from './ViewMessage'
  import Sidebar from './sidebar'

  export default {
    props: {
      messages: {
        type: Array,
        require: true
      }
    },
    data() {
      return {
        history: [
          {
            tag: 'app-inbox',
            title: 'inbox',
            data: {
              messages: null
            }
          }
        ]
      }
    },
    created() {
      eventBus.$on('changeView', (data) => {
        let temp = [{
          tag: data.tag,
          title: data.title,
          data: data.data || {}
        }];

        this.history = temp.concat(this.history.splice(0));


      });
    },
    computed: {
      currentView() {
        let current = this.history[0];
        current.data.messages = this.messages;
        return current;
      },
      prevView() {
        return typeof this.history[1] !== 'undefined'? this.history[1]: null;
      }
    },
    components: {
      appInbox: Inbox,
      appSend: Send,
      appTrash: Trash,
      appViewMessage: ViewMessage,
      appImportant: Important,
      appSidebar: Sidebar
    },
    name: "content"
  }
</script>

<style scoped>

</style>
