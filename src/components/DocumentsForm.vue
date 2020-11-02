<template>
    <form class="registration-form" @submit.prevent="onSubmit">
        <div class="formContainer">
            <div class="titleBlock">
                <h2>Введите данные о документе</h2>
            </div>
            <div class="contentBlock">
                <div class="formItem" :class="{ errorInput: $v.document.$error }">
                    <label for="document">Тип документа:</label>
                    <select id="document" v-model="document" :class="{error: $v.document.$error}" @blur="$v.document.$touch()">
                        <option value="" disabled selected>Выберите тип документа</option>
                        <option>Паспорт</option>
                        <option>Свидетельство о рождении</option>
                        <option>Вод. удостоверение</option>
                    </select>
                    <br>
                    <div class="error" v-if="!$v.document.required">Обязательное поле</div>
                </div>

                <div>
                    <label for="series" >Серия: </label>
                    <input id="series" type="number" v-model="series" placeholder="Серия документа">
                </div>
                <div>
                    <label for="documentNumber" >Номер: </label>
                    <input id="documentNumber" type="number" v-model="documentNumber" placeholder="Номер документа">
                </div>
                <div>
                    <label for="issuedBy" >Кем выдан: </label>
                    <input id="issuedBy" v-model="issuedBy" placeholder="Кем выдан документ">
                </div>
                <div class="formItem" :class="{ errorInput: $v.issueDate.$error }">
                    <label for="issueDate" >Дата выдачи: </label>
                    <input id="issueDate" placeholder="01.01.01" v-mask="'##.##.##'" v-model= "issueDate" :class="{ error: $v.issueDate.$error }" @change="$v.issueDate.$touch()">
                    <br>
                    <div class="error" v-if="!$v.issueDate.required">Обязательное поле</div>
                </div>
                <div>
                    <button class="finalButton" type="submit" value="Далее">Завершить регестрацию</button>
                    <p class="typo__p_1" v-if="submitStatus === 'OK'">Данные успешно отправлены!</p>
                    <p class="typo__p" v-if="submitStatus === 'ERROR'">Пожалуйста, заполните обязательные поля.</p>
                    <p class="typo__p_1" v-if="submitStatus === 'PENDING'">Отправка данных...</p>
                </div>


            </div>
        </div>
    </form>
</template>

<script>
    import {mask} from 'vue-the-mask'
    import {required} from 'vuelidate/lib/validators'

    export default {

        data() {
            return {
                document: '',
                series: '',
                documentNumber: '',
                issuedBy: '',
                issueDate: '',
                submitStatus: null
            }
        },
        validations: {
            document: {required},
            issueDate: {required}
        },
        directives: {mask},
        methods: {
            onSubmit() {
                this.$v.$touch()
                if (this.$v.$invalid) {
                    this.submitStatus = 'ERROR'
                } else {
                    this.submitStatus = 'PENDING'
                    if(this.document.trim()) {
                        const newClientDocumentData = {
                            id: Date.now(),
                            document: this.document,
                            series: this.series,
                            documentNumber: this.documentNumber,
                            issuedBy: this.issuedBy,
                            issueDate: this.issueDate
                        }

                        this.$emit('add-client-document-data', newClientDocumentData)
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
    .finalButton
        margin-left: 25%
</style>