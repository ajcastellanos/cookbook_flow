<template>
    <v-app>
        <div v-if="account_type === 'owner'" class="container">
            <div class="row">
                <div class="col-12">
                    <button class="btn-normal" @click="dialogCollaborator = true">Create Collaborator</button>
                    <span> </span>
                    <collaborator-dialog
                                       @addCollaborator="addCollaborator"
                                       @closeDialog="closeCollaboratorDialogHandler()"
                                       :dialogCollaborator="dialogCollaborator">
                    </collaborator-dialog>
                    <br>
                </div>
            </div>
         <div class="row">
             <div class="col-12 text-center">

                 <v-simple-table>
                     <template v-slot:default>
                         <thead>
                         <tr>
                             <th class="text-left">First</th>
                             <th class="text-left">Last</th>
                             <th class="text-left">Email</th>
                             <th class="text-left">Invitation Status</th>
                             <th class="text-left">Actions</th>
                         </tr>
                         </thead>
                         <tbody>
                         <tr v-for="collaborator in collaborators" :key="collaborator.ID">
                             <td>{{ collaborator.first }}</td>
                             <td>{{ collaborator.last }}</td>
                             <td>{{ collaborator.email }}</td>
                             <td><span class="badge" :class="collaborator.status === 'Accepted' ? 'badge-success' : 'badge-secondary'">{{ collaborator.status }}</span></td>
                             <td>
                                 <button class="btn-normal" @click="removeCollaborator(collaborator.ID)">Remove</button>
                                 <!--<button class="btn-normal" @click="sendInvitationLink(collaborator.ID)">Resend Invitation</button>-->
                             </td>
                         </tr>
                         </tbody>
                     </template>
                 </v-simple-table>


             </div>
         </div>
        </div>
    </v-app>
</template>

<script>
    const axios = require('axios');

    export default {
        data () {
            return {
                dialogCollaborator: false,
                collaborators:[],
                account_type: 0,
            }
        },
        created(){
            this.account_type = parameters.account_selected.account_type;
            this.getCollaborators();
        },
        methods:{
            closeCollaboratorDialogHandler(){
                this.dialogCollaborator = false;
            },
            getCollaborators(){
                const formData = new FormData();
                formData.append('action', 'get_collaborators');
                formData.append('user_id', parameters.account_selected.id);
                axios.post(parameters.ajax_url, formData)
                    .then( response => {
                        if(response.data.success){
                            this.collaborators =  response.data.collaborators;
                        }else{
                            toastr.warning('We could not get the collaborators', 'Error');
                        }
                    });
            },
            removeCollaborator(id){
                if(confirm('Are you sure you want to remove the collaborator?')){
                    const formData = new FormData();
                    formData.append('action', 'remove_collaborator');
                    formData.append('collaborator_id', id);
                    formData.append('owner_id', parameters.account_selected.id);
                    axios.post(parameters.ajax_url, formData)
                        .then( response => {
                            if(response.data.success){
                                this.collaborators =  response.data.collaborators;
                                toastr.success(response.data.msg, 'Success');
                                this.deleteCollaborator(id);
                            }else{
                                toastr.warning(response.data.msg, 'Error');
                            }
                    });
                }

            },
            sendInvitationLink(id){
                alert('Send invitation Link to collaborator ' + id + ' no implemented yet');
            },
            addCollaborator(collaborator){
                this.collaborators.push(collaborator);
            },
            deleteCollaborator(id){
                this.collaborators = this.collaborators.filter(function(collaborator){
                    return collaborator.ID !== id
                });
            }
        }

    }
</script>


<style scoped>

</style>
