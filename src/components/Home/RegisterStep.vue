<template>
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
                    <v-text-field label="Primeiro Nome" outlined variant="outlined" class="mt-10" />
                    <v-text-field label="Segundo Nome" outlined variant="outlined" />
                    <v-text-field label="Email" outlined type="email" variant="outlined" />
                    <v-text-field label="Senha" outlined type="password" variant="outlined" />
                    <v-btn color="#64e3ce" block class="mt-5 mb-10">Entrar</v-btn>
                </v-col>
            </v-row>
        </v-col>
    </v-row>
</template>

<style scoped>
.rounded-br-x1 {
    background-color: #64e3ce;
    border-bottom-right-radius: 300px !important
}
</style>