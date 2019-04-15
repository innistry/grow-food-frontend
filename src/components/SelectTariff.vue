<template>
    <b-form-group id="input-group-3" label="Food:" label-for="input-3">
        <b-form-select
                id="input-3"
                v-model="form.food"
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
            state.options = response.data
            state.loaded = true
        }
    }

    export default {
        props: {

        },

        computed: {
            options() {
                return state.options
            },

            loading() {
                return state.loaded === false
            }
        },

        async created() {
            await fetch()
        }
    }
</script>
