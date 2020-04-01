<template>
    <v-container>
        Statistiques {{ country }}
    </v-container>
</template>

<script>
export default {
    name : 'Stats',
    props : {
        country : {
            type        : String,
            required    : true,
        },
    },
};
</script>
