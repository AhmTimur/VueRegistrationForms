<template>
    <div id="app">
        <RegistrationForm v-bind:clients='clients' @add-client="addClient"/>
        <AdressForm v-bind:clientsAddresses='clientsAddresses' @add-client-address="addClientAddress"/>
        <DocumentsForm v-bind:clientDocumentData='clientDocumentData' @add-client-document-data="addClientDocumentData"/>

        <div v-if="clients.length">Личные данные пользователей: <br>{{clients}}</div>
        <div v-if="clientsAddresses.length">Адреса клиентов: <br>{{clientsAddresses}}</div>
        <div v-if="clientDocumentData.length">Данные о документе: <br>{{clientDocumentData}}</div>
    </div>
</template>

<script>
    import AdressForm from "@/components/AdressForm";
    import DocumentsForm from "@/components/DocumentsForm";
    import RegistrationForm from "@/components/RegistrationForm";

    export default {
        name: 'App',
        data() {
            return {
                clients: [],
                clientsAddresses: [],
                clientDocumentData: []
            }
        },
        methods: {
            addClient(clientData) {
                this.clients.push(clientData)
            },
            addClientAddress(addressData) {
                this.clientsAddresses.push(addressData)
            },
            addClientDocumentData(documentData) {
                this.clientDocumentData.push(documentData)
            }
        },
        components: {
            AdressForm,
            DocumentsForm,
            RegistrationForm
        }
    }

</script>

<style lang="sass">
    input[type='number']
        -moz-appearance: textfield
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button
        -webkit-appearance: none
    .contentBlock
        padding-left: 10%
        @media screen and (max-width: 1600px)
            padding: 15px
    .contentBlock div
        padding: 5px
    .button
        margin-left: 40%
    .formContainer
        box-shadow: 0.4em 0.4em 5px rgba(122,122,122,0.5)
        border: 2px solid grey
        border-radius: 5px
        width: 25%
        margin-left: 35%
        margin-bottom: 10px
        @media screen and (min-width: 1600px)
            width: 25%
    @media screen and (max-width: 1480px)
        .formItem input
            max-width: 100px
            text-align: center
        .formItem select
            max-width: 150px
        .phoneNumberDiv
            padding: 5px 0px 5px 0px
        .formItem
            padding-left: 20px
        .finalButton
            margin-left: 15%
        .addressBlock
            padding-left: 20%
    @media screen and (max-width: 1305px)
        .contentBlock div
            padding-left: 10px
</style>
