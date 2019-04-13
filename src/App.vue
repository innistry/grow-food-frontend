<template>
  <div class="container">
    <img alt="Vue logo" src="@/assets/logo.png">
    <input type="text" />

    <b-alert show>Default Alert</b-alert>

    <div>
      <b-button @click="acha" variant="outline-primary">Button</b-button>
    </div>

    <div>
      {{ axe }}
    </div>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        props: {
            msg: String
        },
        data() {
            return {
                'axe': ''
            }
        },
        methods: {
            async acha() {
                try {
                    this.axe = await axios.get('http://127.0.0.1:8000/api')

                } catch (error) {
                    return false
                }
            }
        }
    }
</script>
