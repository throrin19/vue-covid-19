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
    watch : {
        country(value) {
            console.log('change country', value);
        },
    },
};
</script>
