<template>
    <b-container class="pt-5">
        <b-img center src="https://picsum.photos/125/125/?image=58" alt="Center image"></b-img>

        <b-form @submit.prevent="onSubmit" @reset="onReset">
            <b-form-group
                    label="Куда нам звонить, если что?"
                    label-for="input-phone"
                    description="Никто не узнает про ваш телефон."
            >
                <b-form-input
                        id="input-phone"
                        v-model="form.phone"
                        type="tel"
                        required
                        placeholder="Введите телефон"
                ></b-form-input>
            </b-form-group>

            <b-form-group label="Как к вам обращаться?" label-for="input-name">
                <b-form-input
                        id="input-name"
                        v-model="form.name"
                        type="text"
                        required
                        placeholder="Введите имя"
                ></b-form-input>
            </b-form-group>

            <SelectTariff v-model="tariff" />

            <b-form-group label="Выберите первый день доставки" label-for="input-started_at">
                <b-form-input
                        id="input-started_at"
                        v-model="form.started_at"
                        type="date"
                        required
                ></b-form-input>
            </b-form-group>

            <b-form-group label="Город доставки:" label-for="input-location">
                <b-form-select
                        id="input-location"
                        v-model="form.location_id"
                        :options="locations"
                        :disabled="Object.keys(tariff).length === 0"
                        required
                ></b-form-select>
            </b-form-group>

            <b-form-group label="Уточните адрес доставки" label-for="input-address">
                <b-form-input
                        id="input-address"
                        v-model="form.address"
                        type="text"
                        required
                        placeholder="Введите адрес"
                ></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Сделать заказ</b-button>
        </b-form>

        <div class="mt-5">
            <h4>Заказы</h4>
            <b-table striped hover :items="ordersTable"></b-table>
        </div>
    </b-container>
</template>

<script>
    import SelectTariff from '@/components/SelectTariff'

    import axios from 'axios'

    export default {
        components: {
            SelectTariff
        },
        data() {
            return {
                orders: [],
                form: {
                    name: '',
                    phone: '',
                    tariff_id: null,

                    started_at: '',

                    location_id: '',
                    address: '',
                },
                tariff: {}
            }
        },
        computed: {
            ordersTable() {
                return this.orders.map((order) => order);
            },
            locations() {
                return this.tariff && this.tariff.locations ? this.tariff.locations.map((location) => ({
                    value: location.id,
                    text: location.city
                })) : [];
            }
        },
        watch: {
            tariff: {
                handler: function () {
                    this.form.tariff_id = this.tariff.id
                },
                deep: true
            }
        },
        methods: {
            async onSubmit() {
                try {
                    const response = await axios.post('api/orders', this.form);

                    if (response.data.data) {
                        this.orders.unshift(response.data.data)
                        this.onReset()
                    }
                } catch (error) {
                    return false
                }
            },
            onReset() {
                this.tariff = {}
                this.form = {
                    name: '',
                    phone: '',
                    tariff_id: null,

                    started_at: '',

                    location_id: '',
                    address: '',
                }
            }
        },
        async created() {
            try {
                const response = await axios.get('api/orders');
                this.orders = response.data.data;
            } catch (error) {
                return false
            }
        }
    }
</script>
