<template>
    <DashBoard/>
</template>

<script>
import { defineComponent } from 'vue';

import DashBoard from '../components/DashBoard/DashBoard.vue';

export default defineComponent({
    name: 'DashBoardView',

    components: {
        DashBoard
    }
});
</script>