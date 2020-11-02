<template>
    <form class="registration-form" @submit.prevent="onSubmit">
        <div class="formContainer">
            <div class="titleBlock">
                <h2>Личные данные</h2>
            </div>
           <div class="contentBlock registration">
               <div class="formItem" :class="{ errorInput: $v.name.$error }">
                   <label for="name">Имя: </label>
                   <input id="name" placeholder="Имя" v-model= "name" :class="{ error: $v.name.$error }" @change="$v.name.$touch()">
                   <br>
                   <div class="error" v-if="!$v.name.required">Обязательное поле</div>
               </div>
               <div class="formItem" :class="{ errorInput: $v.surname.$error }">
                   <label for="surname">Фамилия: </label>
                   <input id="surname" placeholder="Фамилия" v-model="surname" :class="{ error: $v.surname.$error }"
                          @change="$v.surname.$touch()">
                   <br>
                   <div class="error" v-if="!$v.surname.required">Обязательное поле</div>
               </div>
               <div>
                   <label for="patronymic">Отчество: </label>
                   <input id="patronymic" v-model="patronymic" placeholder="Отчество">
               </div>
               <div class="formItem" :class="{ errorInput: $v.birthDate.$error }">
                   <label for="birthDate">Дата рождения: </label>
                   <input id="birthDate"  type="date" v-model="birthDate" @blur="$v.birthDate.$touch()">
                   <br>
                   <div class="error" v-if="!$v.birthDate.required">Обезательное поле</div>
               </div>
               <div class="formItem" :class="{ errorInput: $v.phoneNumber.$error }">
                   <label>Номер телефона: </label>
                   <div class="phoneNumberDiv">
                       <span>+7</span>
                       <input type="tel" id="phoneNumber" v-mask="'###-###-####'" v-model="phoneNumber"
                              placeholder="(900)-123-45-67" :class="{ error: $v.phoneNumber.$error }" @change="$v.phoneNumber.$touch()">
                   </div>
                   <br>
                   <div class="error" v-if="!$v.phoneNumber.required">Укажите номер телефона</div>
               </div>
               <div>
                   <label>Пол:</label>
                   <input type="radio" id="man" value="Мужчина" v-model="sex">
                   <label for="man">Мужчина</label>
                   <input type="radio" id="woman" value="Женщина" v-model="sex">
                   <label for="woman">Женщина</label>
               </div>
               <div class="formItem" :class="{ errorInput: $v.clientsGroup.$error }">
                   <label for="clientsGroup">Группа клиентов:</label>
                   <select id="clientsGroup" v-model="clientsGroup" :class="{error: $v.clientsGroup.$error}"
                           @blur="$v.clientsGroup.$touch()">
                       <option value="" disabled selected>Выберите группу</option>
                       <option>VIP</option>
                       <option>Проблемные</option>
                       <option>ОМС</option>
                   </select>
                   <br>
                   <div class="error" v-if="!$v.clientsGroup.required">Обезательное поле</div>
               </div>
               <div>
                   <label for="therapist">Лечащий врач:</label>
                   <select id="therapist" v-model="therapist">
                       <option value="" disabled selected>Выберите фамилию лечащего врача</option>
                       <option>Иванов</option>
                       <option>Захаров</option>
                       <option>Чернышева</option>
                   </select>
               </div>
               <div>
                   <label for="doNotSendSMS">Не отправлять СМС</label>
                   <input type="checkbox" id="doNotSendSMS">
               </div>
               <div>
                   <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">Далее</button>
                   <p class="typo__p_1" v-if="submitStatus === 'OK'">Регистрация прошла успешно!</p>
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
        name: 'RegistrationForm',
        data() {
            return {
                name: '',
                surname: '',
                patronymic: '',
                birthDate: '',
                phoneNumber: '',
                sex: '',
                clientsGroup: '',
                therapist: '',
                doNotSendSMS: '',
                submitStatus: null
            }
        },
        validations: {
            name: {required},
            surname: {required},
            birthDate: {required},
            phoneNumber: {required},
            clientsGroup: {required}
        },
        directives: {mask},
        methods: {
            onSubmit() {
                this.$v.$touch()
                if (this.$v.$invalid) {
                    this.submitStatus = 'ERROR'
                } else {
                    this.submitStatus = 'PENDING'
                    if (this.name.trim()) {
                        const newClient = {
                            id: Date.now(),
                            name: this.name,
                            surname: this.surname,
                            birthDate: document.getElementById('birthDate').value,
                            patronymic: this.patronymic,
                            sex: this.sex,
                            phoneNumber: '+7-' + this.phoneNumber,
                            clientsGroup: this.clientsGroup,
                            therapist: this.therapist,
                            doNotSendSMS: document.getElementById('doNotSendSMS').checked
                        }

                        this.$emit('add-client', newClient)
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
    .formItem
        .error
            display: none
        &.errorInput
            .error
                display: inline
                font-size: 14px
    .titleBlock
        background-color: #99ccff
    .titleBlock h2
        text-align: center
        margin: 0
        padding: 5px
    .error, .typo__p
        color: red
    .formItem input, #phoneNumberPrefix
        &.error
            border-color: red
    .typo__p_1
        color: black
    .phoneNumberDiv
        display: inline
    @media screen and (max-width: 1480px)

</style>