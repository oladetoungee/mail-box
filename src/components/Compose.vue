<template>
    <div>
        <a href="#composeModal" data-toggle="modal" class="btn btn-compose" @click="popUp = true">Compose</a>

        <div aria-hidden="true" role="dialog" tabindex="-1" id="composeModal" class="modal fade" v-if="popUp">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <button aria-hidden="true" data-dismiss="modal" class="close" type="button" @click="popUp=false" >&times; </button>
                        <h4 class="modal-title">New Message</h4>
                    </div>

                    <div class="modal-body">
                        <form @submit.prevent="sendMessage" role="form" class="form-horizontal">
                            <div class="form-group">
                                <label class="col-lg-2 control-label" for="subject">Subject</label>
                                <div class="col-lg-10">
                                    <input type="text" v-model="message.subject" id="subject" class="form-control">
                                </div>
                            </div>

                            <div class="form-group">
                                <label class="col-lg-2 control-label" for="message">Message</label>
                                <div class="col-lg-10">
                                    <textarea v-model="message.content" rows="10" cols="30" class="form-control" id="message"></textarea>
                                </div>
                            </div>

                            <div class="form-group">
                                <div class="col-lg-offset-2 col-lg-10">
                                    <button class="btn btn-send" type="submit">Send</button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import { eventBus } from "../main";
    import moment from 'moment'

    export default {
        data() {
            return {
                popUp: false,
                message: {
                    subject: '',
                    content: '',
                    
                }
            }
        },
        methods: {
            sendMessage() {
                eventBus.$emit('sentMessage', {
                   message: {
                       subject: this.message.subject,
                       content: this.message.content,
                       isDeleted: false,
                       type: 'outgoing',
                       date: moment(),
                       from: {
                           name: 'Oladetoun Gbemisola',
                           email: 'demilad1998@gmail.com'
                       },
                       attachments: []
                   }
                })
            }
        }
    }
</script>