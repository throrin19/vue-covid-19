<template>
    <v-navigation-drawer
        app
        permanent
        clipped>

        <v-autocomplete
            v-model="selectedCountry"
            :items="countries"
            class="ma-3"
            item-value="Slug"
            item-text="Country"
            label="Country"
            hint="Select country to see details"
            persistent-hint
            />

        <v-list
            dense
            shaped>
            <v-list-item to="/">
                <v-list-item-content>
                    <v-list-item-title>Home</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
            <v-subheader>Top 5 new confirmed</v-subheader>
            <v-list-item
                v-for="country in topNewConfirmed"
                :key="`confirmed-${country.Slug}`"
                :to="`/stats/${country.Slug}`">
                <v-list-item-content>
                    <v-list-item-title v-text="country.Country"></v-list-item-title>
                </v-list-item-content>
                <v-list-item-icon>
                    <v-chip
                        color="amber"
                        class="black--text"
                        x-small>
                        +{{ country.NewConfirmed }}
                    </v-chip>
                </v-list-item-icon>
            </v-list-item>

            <v-subheader>Top 5 new death</v-subheader>
            <v-list-item
                v-for="country in topNewDeath"
                :key="`death-${country.Slug}`"
                :to="`/stats/${country.Slug}`">
                <v-list-item-content>
                    <v-list-item-title v-text="country.Country"></v-list-item-title>
                </v-list-item-content>
                <v-list-item-icon>
                    <v-chip
                        color="red"
                        x-small>
                        +{{ country.NewDeaths }}
                    </v-chip>
                </v-list-item-icon>
            </v-list-item>

            <v-subheader>Top 5 new recovered</v-subheader>
            <v-list-item
                v-for="country in topNewRecovered"
                :key="`recovered-${country.Slug}`"
                :to="`/stats/${country.Slug}`">
                <v-list-item-content>
                    <v-list-item-title v-text="country.Country"></v-list-item-title>
                </v-list-item-content>
                <v-list-item-icon>
                    <v-chip
                        color="green"
                        x-small>
                        +{{ country.NewRecovered }}
                    </v-chip>
                </v-list-item-icon>
            </v-list-item>
        </v-list>
    </v-navigation-drawer>
</template>

<script>
import orderBy from 'lodash/orderBy';
import { get } from 'axios';

export default {
    name : 'Drawer',
    data() {
        return {
            selectedCountry : null,
            countries       : [],
        };
    },
    computed : {
        topNewConfirmed() {
            const sorted = orderBy(this.countries, ['NewConfirmed'], ['desc']);

            return sorted.slice(0, 5);
        },
        topNewDeath() {
            const sorted = orderBy(this.countries, ['NewDeaths'], ['desc']);

            return sorted.slice(0, 5);
        },
        topNewRecovered() {
            const sorted = orderBy(this.countries, ['NewRecovered'], ['desc']);

            return sorted.slice(0, 5);
        },
    },
    watch : {
        selectedCountry(value) {
            this.$router.push(`/stats/${value}`);
        },
    },
    async mounted() {
        await this.getSummary();
    },
    methods : {
        async getSummary() {
            const result = await get('https://api.covid19api.com/summary');

            this.countries = result.data.Countries;
        },
    }
}
</script>
