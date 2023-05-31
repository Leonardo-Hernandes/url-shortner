<template>
    <v-app-bar :elevation="0">
        <v-col cols="3">
            <v-app-bar-title class="ml-10">
                <p class="font-weight-bold">Urls</p>
            </v-app-bar-title>
        </v-col>
        <v-col cols="6" class="center">
            <v-icon class="mr-5">mdi-magnify</v-icon>
            <v-text-field v-model="search" label="Pesquisar" single-line hide-details variant="outlined"></v-text-field>
            <v-btn icon class="ml-3">
                <v-icon @click="newLinkDialog = true">mdi-plus</v-icon>
            </v-btn>
        </v-col>
        <v-col cols="3" style="display: flex; justify-content: end;">
            <v-btn icon>
                <v-icon @click.stop="drawer = !drawer">mdi-account</v-icon>
            </v-btn>
        </v-col>



    </v-app-bar>
    <div style="background-color : #F4F6FA; height: 100%;">
        <v-container>
            <v-row align="center" justify="center">
                <v-col class="mt-10 mb-10" cols="12" sm="9">
                    <v-row>
                        <v-col cols="12">
                            <p class="font-weight-bold">Status</p>
                        </v-col>
                        <v-row>
                            <v-col>
                                <p class="font-weight-bold text-h4 text-center">{{ this.links.length }}</p>
                                <p class="font-weight-medium text-center">Links</p>
                            </v-col>
                            <v-col>
                                <p class="font-weight-bold text-h4 text-center">{{ this.views }}</p>
                                <p class="font-weight-medium text-center">Visualizações</p>
                            </v-col>
                            <v-col>
                                <p class="font-weight-bold text-h4 text-center">{{ this.views }}</p>
                                <p class="font-weight-medium text-center">Acessos</p>
                            </v-col>
                        </v-row>


                    </v-row>
                    <v-col cols="12">
                        <LinkCard v-on:update="getLinks()" v-for="item in links"
                            :key="item.id" :item="item" />
                    </v-col>
                </v-col>

            </v-row>
        </v-container>
    </div>
    <v-dialog v-model="newLinkDialog" width="40rem">
        <v-card>
            <v-card-text class="mt-5">
                Adicionar um novo Link
            </v-card-text>
            <v-form v-model="valid">
                <v-text-field label="Identificador" v-model="identifier" outlined type="email" variant="outlined"
                    class="mt-4 mb-1 ml-3 mr-3" />
                <v-text-field label="Url *" v-model="url" :rules="urlRules" outlined type="email" variant="outlined"
                    class="mt-1 mb-1 ml-3 mr-3" />
            </v-form>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red-1" variant="text" @click="newLinkDialog = false">
                    Cancelar
                </v-btn>
                <v-btn color="green-darken-1" variant="text" @click="handleNewLink()">
                    Confirmar
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <v-navigation-drawer v-model="drawer" location="right" temporary>
        <v-col cols="12" style="display: flex; justify-content: center;">
            <v-btn variant="text" @click="handleLogout()">
                Sair
            </v-btn>
        </v-col>
    </v-navigation-drawer>

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
import LinkCard from "./LinkCard.vue";
import './styles.css';

import api from '../../config/api';

export default {
    data: () => ({
        newLinkDialog: false,
        valid: false,
        drawer: false,
        isLoading: false,
        identifier: '',
        snackbar: false,
        text: '',
        user: {},
        url: '',
        searh: '',
        views: 0,
        links: {},
        token: localStorage.getItem('token'),
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

    components: {
        LinkCard,
    },

    async mounted() {
        const user = localStorage.getItem('user');
        this.user = JSON.parse(user);

        if (!this.user)
            window.location.href = "/"

        this.getLinks()
    },

    methods: {
        async handleNewLink() {
            if (!this.valid) {
                this.snackbar = true;
                this.text = "Por favor, preencha o campo Url e tente novamente!"
            } else {
                var config = {
                    headers: { Authorization: `Bearer ${this.token}` }
                };

                api.post("/link", {
                    'url': this.url,
                    'identifier': this.identifier ? this.identifier : null
                }, config).then(() => {
                    this.getLinks()
                    this.newLinkDialog = false
                })
                    .catch(() => {
                        console.log("error")
                    });
            }
        },

        

        handleLogout() {
            localStorage.removeItem('user');
            localStorage.removeItem('token');

            window.location.href = "/"
            this.drawer = false
        },

        countViews() {
            this.views = 0
            for (let item of this.links) {
                this.views = this.views + parseInt(item.views)
            }
        },

        async getLinks() {
            await api
                .get(`/link/search/${this.user.id}`)
                .then((res) => {
                    this.links = res.data;
                })
                .catch((error) => {
                    console.log(error)
                });
            this.countViews()
        }
    },


}

</script>