<template>
    <v-card text class="mt-7 mb-7">
        <v-row style="margin: 0.5rem">
            <v-col cols="8">
                <p class="font-weight-bold"> Google</p>
                <a href="google.com"> google.com</a>
            </v-col>
            <v-col cols="4" class="center">
                <p class=" ml-2 mr-1 mt-2">20</p>
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
            <v-text-field label="Email" v-model="identifier" outlined type="email" variant="outlined"
                class="mt-4 mb-1 ml-3 mr-3" />
            <v-text-field label="Email" v-model="url" outlined type="email" variant="outlined"
                class="mt-1 mb-1 ml-3 mr-3" />
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red-1" variant="text" @click="editDialog = false">
                    Cancelar
                </v-btn>
                <v-btn color="green-darken-1" variant="text" @click="handleEdit()">
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
export default {
    data: () => ({
        editDialog: false,
        deleteDialog: false,
        snackbar: false,
        identifier: '',
        url: '',
        text: '',
    }),

    methods: {
        copyToClipboard() {
            navigator.clipboard.writeText("google.com");
            this.snackbar = true;
            this.text = "Link copiado com sucesso!"
        },

        handleEdit() {
            console.log("edit")
            this.editDialog = false
        },

        handleDelete() {
            console.log("delete")
            this.deleteDialog = false
        }
    }
}
</script>