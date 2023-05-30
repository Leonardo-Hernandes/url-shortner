<template>
    <v-form v-model="valid">
        <v-row style="height: 100%;">
            <v-col cols="12" sm="6">
                <v-card-text class="white--text mt-12">
                    <h2 class="text-center mb-5">Já possui uma conta?</h2>
                    <h5 class="text-center">Faça login para que você possa encurtar suas urls e <br>
                        administrar as já existentes!
                    </h5>
                </v-card-text>
                <v-row align="center" justify="center">
                    <v-col cols="12" sm="8">
                        <v-text-field label="Email" v-model="email" :rules="emailRules" outlined type="email"
                            variant="outlined" class="mt-10 mb-3" />
                        <v-text-field label="Senha" v-model="password" :rules="passwordRules" outlined type="password"
                            variant="outlined" />
                        <v-btn color="#64e3ce" block class="mt-5" @click="handleSubmit()">{{ isLoading ? "Carregando..." : "Entrar" }}</v-btn>
                    </v-col>
                </v-row>
            </v-col>

            <v-col cols="12" sm="6" class="blue rounded-bl-x1">
                <div style="text-align:center; padding: 180px 0;">
                    <v-card-text class="white--text">
                        <h2 class="text-center mb-5">Ainda não tem uma conta?</h2>
                        <h5 class="text-center">
                            Vamos criar uma conta para que você possa desfrutar dos serviços<br>
                            de urls encurtadas e análise das mesmas 😁
                        </h5>
                    </v-card-text>
                    <div class="text-center">
                        <v-btn tile outlined dark @click="$emit('register')">Cadastrar</v-btn>
                    </div>
                </div>
            </v-col>
        </v-row>
    </v-form>
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
import api from '../../config/api'

export default {
    data: () => ({
        valid: false,
        snackbar: false,
        isLoading: false,
        user: {},
        token: '',
        text: '',
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
        // password: '',
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
                this.isLoading = true;
                const data = {
                    email: this.email,
                    password: this.password
                };
                await api
                    .post("/login", data)
                    .then((res) => {
                        this.user = res.data.user;
                        this.token = res.data.token
                    })
                    .catch(() => {
                        this.snackbar = true;
                        this.text = "Dados inválidos, por favor tente novamente!"
                    });
                this.isLoading = false
            } else {
                this.snackbar = true;
                this.text = "Por Favor preencha todos os campos corretamente!"
            }
        }
    }

}
</script>

<style scoped>
.rounded-bl-x1 {
    background-color: #64e3ce;
    border-bottom-left-radius: 300px !important;
}
</style>