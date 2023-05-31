<template>
    <v-card text class="mt-7 mb-7">
        <v-row style="margin: 0.5rem">
            <v-col cols="8">
                <p class="font-weight-bold">{{ this.item.to }}</p>
                <a :href="this.apiUrl + this.item.to"> {{ this.apiUrl + this.item.to }}</a>
            </v-col>
            <v-col cols="4" class="center">
                <p class=" ml-2 mr-1 mt-2">{{ this.item.views }}</p>
                <v-icon class=" ml-1 mr-2">mdi-poll</v-icon>
                <v-icon class="iconButton ml-2 mr-2" @click="copyToClipboard()">mdi-content-copy</v-icon>
                <v-icon class="iconButton ml-2 mr-2" @click="editDialog = true">mdi-file-edit-outline</v-icon>
                <v-icon class="iconButton ml-2 mr-2" @click="deleteDialog = true">mdi-eraser</v-icon>
            </v-col>
        </v-row>

    </v-card>

    <v-dialog v-model="editDialog" width="auto">
        <v-card width="40rem">
            <v-card-text class="mt-5">
                Edição de Url
            </v-card-text>
            <v-text-field label="Identificador" v-model="identifier" outlined variant="outlined"
                class="mt-4 mb-1 ml-3 mr-3" />
            <v-text-field label="Url *" v-model="url" :rules="urlRules" outlined variant="outlined"
                class="mt-1 mb-1 ml-3 mr-3" />
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red-1" variant="text" @click="editDialog = false">
                    Cancelar
                </v-btn>
                <v-btn color="green-darken-1" variant="text" @click="handleUpdate()">
                    Confirmar
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <v-dialog v-model="deleteDialog" width="auto">
        <v-card>
            <v-card-text class="mt-5">
                Tem certeza que dejesa excluir este link?
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red-1" variant="text" @click="deleteDialog = false">
                    Cancelar
                </v-btn>
                <v-btn color="green-darken-1" variant="text" @click="handleDelete()">
                    Confirmar
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <v-snackbar v-model="snackbar">
        {{ text }}

        <template v-slot:actions>
            <v-btn color="white" variant="text" @click="snackbar = false">
                Close
            </v-btn>
        </template>
    </v-snackbar>
</template>

<script>
import api from '@/config/api';

export default {
    data: () => ({
        editDialog: false,
        deleteDialog: false,
        snackbar: false,
        identifier: '',
        token: localStorage.getItem("token"),
        url: '',
        text: '',
        apiUrl: 'http://127.0.0.1:8000/api/link/redirect/',
        urlRules: [
            value => {
                if (value) return true

                return 'Url é obrigatória.'
            },
            value => {
                if (/^https:\/\//.test(value) || /^http:\/\//.test(value)) return true

                return 'Url inválida, digite novamente.'
            },
        ],
    }),

    props: ['item'],

    methods: {
        copyToClipboard() {
            navigator.clipboard.writeText(`${this.apiUrl + this.item.to}`);
            this.snackbar = true;
            this.text = "Link copiado com sucesso!"
        },

        async handleUpdate() {
            var config = {
                headers: { Authorization: `Bearer ${this.token}` }
            };
            api.put(`/link/${this.item.id}`, {
                "identifier": this.identifier,
                "url": this.url
            }, config).then(() => {
                this.editDialog = false
                this.$emit('update');
            })
                .catch(() => {
                    console.log("error")
                });
        },

        async handleDelete() {
            var config = {
                headers: { Authorization: `Bearer ${this.token}` }
            };

            api.delete(`/link/delete/${this.item.id}`, config).then(() => {
                this.deleteDialog = false
                this.$emit('update');
            })
                .catch(() => {
                    console.log("error")
                });
        },
    }
}
</script>