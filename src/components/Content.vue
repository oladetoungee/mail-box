<template>
   <aside class="lg-side">
       <div class="inbox-head">
           <h3>{{ currentView.title }}</h3>
       </div>
       <keep-alive>
               <component :is="currentView.tag" :data="currentView.data"></component>
       </keep-alive>
   </aside>
</template>
<script>

    import Inbox  from '../components/Inbox.vue'
    import Sent  from '../components/Sent.vue'
    import Important  from '../components/Important.vue'
    import Trash  from '../components/Trash.vue'
    import ViewMessage  from '../components/ViewMessage.vue'
    import { eventBus } from "../main";

    
    export default {
        props: {
            messages: {
                type: Array,
                required: true
            }
        },
        created() {
            eventBus.$on('changeView', (data) => {
                let temp = [{
                    tag: data.tag,
                    title: data.title,
                    data: data.data || {}
                }];

                console.log(temp)
                this.history = temp.concat(this.history.splice(0))
            })
        },
        data() {
            return {
                history: [
                    {
                        tag: 'app-inbox',
                        title: 'Inbox',
                        data: {
                            messages: null
                        }
                    }
                ]
            }
        },
        computed: {
            currentView() {
                let current = this.history[0];
                current.data.messages = this.messages;
                return current;
            },
            previousView() {
                return typeof this.history[1] !== 'undefined' ? this.history[1] : null
            }
        },
        components: {
            appInbox: Inbox,
            appSent: Sent,
            appImportant: Important,
            appTrash: Trash,
            appViewMessage: ViewMessage
        }
    }
</script>