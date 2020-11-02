<template>
    <form class="registration-form" @submit.prevent="onSubmit">
        <div class="formContainer">
            <div class="titleBlock">
                <h2>Введите адрес</h2>
            </div>
            <div class="contentBlock addressBlock">
                <div>
                    <label for="index" >Индекс: </label>
                    <input id="index" type="number" v-model="index" placeholder="Индекс">
                </div>
                <div>
                    <label for="country" >Страна: </label>
                    <input id="country" v-model="country" placeholder="Страна">
                </div>
                <div>
                    <label for="region" >Область: </label>
                    <input id="region" v-model="region" placeholder="Область">
                </div>
                <div class="formItem" :class="{ errorInput: $v.city.$error }">
                    <label for="city">Город: </label>
                    <input id="city" placeholder="Город" v-model= "city" :class="{ error: $v.city.$error }" @change="$v.city.$touch()">
                    <br>
                    <div class="error" v-if="!$v.city.required">Обязательное поле</div>
                </div>
                <div>
                    <label for="street" >Улица: </label>
                    <input id="street" v-model="street" placeholder="Улица">
                </div>
                <div>
                    <label for="house" >Дом: </label>
                    <input id="house" type="number" v-model="house" placeholder="Дом">
                </div>
                <div>
                    <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">Далее</button>
                    <p class="typo__p_1" v-if="submitStatus === 'OK'">Данные успешно отправлены!</p>
                    <p class="typo__p" v-if="submitStatus === 'ERROR'">Пожалуйста, заполните обязательные поля.</p>
                    <p class="typo__p_1" v-if="submitStatus === 'PENDING'">Отправка данных...</p>
                </div>
            </div>
        </div>
    </form>
</template>

<script>
    import {required} from 'vuelidate/lib/validators'

    export default {
        data() {
            return {
                index: '',
                country: '',
                region: '',
                city: '',
                street: '',
                house: '',
                submitStatus: null
            }
        },
        validations: {
            city: {required}
        },
        methods: {
            onSubmit() {
                this.$v.$touch()
                if (this.$v.$invalid) {
                    this.submitStatus = 'ERROR'
                } else {
                    this.submitStatus = 'PENDING'
                    if(this.city.trim()) {
                        const newClientAddress = {
                            id: Date.now(),
                            index: this.index,
                            country: this.country,
                            region: this.region,
                            city: this.city,
                            street: this.street,
                            house: this.house
                        }

                        this.$emit('add-client-address', newClientAddress)
                    }
                    setTimeout(() => {
                        this.submitStatus = 'OK'
                    }, 500)
                }

            }
        }
    }
</script>

<style lang="sass">
    input[type='number']
        -moz-appearance: textfield
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button
        -webkit-appearance: none
    .formItem
        .error
            display: none
        &.errorInput
            .error
                display: inline
                font-size: 14px
</style>