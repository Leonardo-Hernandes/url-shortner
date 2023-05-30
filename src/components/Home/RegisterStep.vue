<template>
    <v-form v-model="valid">
        <v-row>
            <v-col cols="12" sm="6" class="blue rounded-br-x1">
                <div style="text-align: center; padding: 180px 0">
                    <v-card-text class="white--text">
                        <h2 class="text-center mb-5">Já possui uma conta?</h2>
                        <h5 class="text-center">Faça login para que você possa encurtar suas urls e <br>
                            administrar as já existentes!
                        </h5>
                    </v-card-text>
                    <div class="text-center">
                        <v-btn tile outlined dark @click="$emit('login')">Login</v-btn>
                    </div>
                </div>
            </v-col>
            <v-col cols="12" sm="6">
                <v-card-text class="white--text mt-12">
                    <h2 class="text-center mb-5">Criar uma nova conta</h2>
                    <h5 class="text-center">Preencha corretamente os campos para que possamos criar <br>
                        a sua conta!
                    </h5>
                </v-card-text>
                <v-row align="center" justify="center">
                    <v-col cols="12" sm="8">
                        <v-text-field label="Primeiro Nome" v-model="firstname" :rules="firstnameRules" outlined
                            variant="outlined" class="mt-10 mb-2" />
                        <v-text-field label="Segundo Nome" v-model="surname" :rules="surnameRules" outlined
                            variant="outlined" class="mb-2" />
                        <v-text-field label="Email" v-model="email" :rules="emailRules" outlined type="email"
                            variant="outlined" class="mb-2" />
                        <v-text-field label="Senha" v-model="password" :rules="passwordRules" outlined type="password"
                            variant="outlined" class="mb-2" />
                        <v-btn color="#64e3ce" block class="mt-5 mb-10" @click="handleSubmit()">Entrar</v-btn>
                    </v-col>
                </v-row>
            </v-col>
        </v-row>
        <v-snackbar v-model="snackbar">
        {{ text }}

        <template v-slot:actions>
            <v-btn color="white" variant="text" @click="snackbar = false">
                Close
            </v-btn>
        </template>
    </v-snackbar>
    </v-form>
</template>

<script>
import api from "../../config/api";

export default {
    data: () => ({
        valid: false,
        isLoading: false,
        snackbar: false,
        text: '',
        firstname: '',
        firstnameRules: [
            value => {
                if (value) return true

                return 'Primeiro nome é obrigatório.'
            },
        ],
        surname: '',
        surnameRules: [
            value => {
                if (value) return true

                return 'Segundo nome é obrigatório.'
            },
        ],
        email: '',
        emailRules: [
            value => {
                if (value) return true

                return 'E-mail é obrigatório.'
            },
            value => {
                if (/.+@.+\..+/.test(value)) return true

                return 'E-mail inválido, digite novamente.'
            },
        ],
        password: '',
        passwordRules: [
            value => {
                if (value) return true

                return 'Senha obrigatória.'
            },
        ],
    }),
    methods: {
        async handleSubmit() {
            if (this.valid) {
                const data = this.makePayload();

                this.isLoading = true;
                await api
                    .post("/user/register", data)
                    .then(() => {
                        this.snackbar = true;
                        this.text = "Usuário cadastrado com sucesso!"
                        this.$emit('login')
                    })
                    .catch((error) => {
                        this.snackbar = true;
                        this.text = error.response.data
                    });
                this.isLoading = false
            } else {
                this.snackbar = true;
                this.text = "Por Favor preencha todos os campos corretamente!"
            }
        },

        makePayload() {
            const payload = {
                name: this.firstname + ' ' + this.surname,
                email: this.email,
                password: this.password
            }

            return payload;
        }
    }
}
</script>

<style scoped>
.rounded-br-x1 {
    background-color: #64e3ce;
    border-bottom-right-radius: 300px !important
}
</style>