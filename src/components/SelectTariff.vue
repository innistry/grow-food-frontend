<template>
    <b-form-group label="Тариф" label-for="input-tariff">
        <b-form-select
                id="input-tariff"
                :value="value"
                v-on="$listeners"
                :options="options"
                required
        ></b-form-select>
    </b-form-group>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'

    const state = new Vue.observable({
        loaded: false,
        options: []
    })

    export async function fetch() {
        if (!state.loaded) {
            let response = await axios.get('api/tariffs')
            state.options = response.data.data
            state.loaded = true
        }
    }

    export default {
        props: {
            value: {
                type: Object,
                default: () => ({})
            }
        },

        computed: {
            options() {
                return state.loaded ? state.options.map((tariff) => ({
                    value: tariff,
                    text: tariff.name
                })) : []
            }
        },

        async created() {
            await fetch()
        }
    }
</script>
