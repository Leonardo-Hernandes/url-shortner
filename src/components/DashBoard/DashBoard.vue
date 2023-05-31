<template>
    <v-app-bar :elevation="0">
        <v-app-bar-title>UrlS</v-app-bar-title>
        <v-btn icon>
            <v-icon>mdi-magnify</v-icon>
        </v-btn>
        <v-btn icon>
            <v-icon>mdi-plus</v-icon>
        </v-btn>
        <v-btn icon>
            <v-icon>mdi-account</v-icon>
        </v-btn>
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
                                <p class="font-weight-bold text-h4 text-center">20</p>
                                <p class="font-weight-medium text-center">Links</p>
                            </v-col>
                            <v-col>
                                <p class="font-weight-bold text-h4 text-center">30</p>
                                <p class="font-weight-medium text-center">Visualizações</p>
                            </v-col>
                            <v-col>
                                <p class="font-weight-bold text-h4 text-center">2</p>
                                <p class="font-weight-medium text-center">Acessos</p>
                            </v-col>
                        </v-row>


                    </v-row>
                    <v-col cols="12">
                        <LinkCard />
                    </v-col>
                </v-col>

            </v-row>
        </v-container>
    </div>
</template>

<script>
import LinkCard from "./LinkCard.vue";
import './styles.css';

export default {
    data: () => ({
    }),

    components: {
        LinkCard,
    }
}

</script>