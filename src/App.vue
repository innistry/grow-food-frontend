<template>
    <b-container class="pt-5">
        <b-img center src="https://picsum.photos/125/125/?image=58" alt="Center image"></b-img>

        <b-form @submit.prevent="onSubmit">
            <b-form-group
                    id="input-group-phone"
                    label="Куда нам звонить, если что?"
                    label-for="input-phone"
                    description="Никто не узнает про ваш телефон, честно."
            >
                <b-form-input
                        id="input-phone"
                        v-model="form.phone"
                        type="phone"
                        required
                        placeholder="Введите телефон"
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Как к вам обращаться?" label-for="input-2">
                <b-form-input
                        id="input-2"
                        v-model="form.name"
                        required
                        placeholder="Введите имя"
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-3" label="Food:" label-for="input-3">
            </b-form-group>

            <b-button type="submit" variant="primary">Сделать заказ</b-button>
        </b-form>

        <!--<SelectTariff />-->

        <div class="mt-5">
            <h4>Заказы</h4>
            <b-spinner v-if="isOrdersLoading" type="grow" label="Spinning"></b-spinner>
            <b-table v-else striped hover :items="ordersTable"></b-table>
        </div>
    </b-container>
</template>

<script>
    import SelectTariff from '@/components/SelectTariff'

    import axios from 'axios'

    export default {
        comments: [
            SelectTariff
        ],
        data() {
            return {
                'axe': '',
                orders: [],
                isOrdersLoading: true,
                form: {
                    name: '',
                    phone: '',
                    tariff_id: 1
                }
            }
        },
        computed: {
            ordersTable() {
                return this.orders.map((order) => order);
            }
        },
        methods: {
            async onSubmit() {
                try {
                    const response = await axios.post('api/orders', this.form);

                    if (response.data.data) {
                        this.orders.unshift(response.data.data)
                    }
                } catch (error) {
                    return false
                }
            }
        },
        async created() {
            try {
                const response = await axios.get('api/orders');

                if (response.data.data) {
                    this.isOrdersLoading = false;
                    this.orders = response.data.data;
                }
            } catch (error) {
                return false
            }
        }
    }
</script>
