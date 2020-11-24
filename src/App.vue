<template>
<div class="wrapper">
    <div class="sample">
        <form v-if="!sendResult">

            <b-progress :value="progressWidth" show-progress animated></b-progress>

            <app-field v-for="(field, i) in info"
                :key="field.id"
                :name="field.name"
                :value="field.value"
                :id="field.id"
                :valid="field.valid"
                @changevalue="onInput(i, $event)"
            >
            </app-field>
            <button class="btn btn-primary" type="button" :disabled="!formReady" @click="onSend">
                Send Data
            </button>
        </form>
        <div v-else="!sendResult">
            <table class="table table-bordered">
                <tr v-for="field in info">
                    <td>{{ field.name }}</td>
                    <td>{{ field.value }}</td>
                </tr>
            </table>
        </div>
    </div>
</div>
</template>
<script>
    import AppField from '@/components/Field';
    import { BProgress } from 'bootstrap-vue'

    export default {
        components: {
            AppField,
            BProgress
        },
        data(){
            return {
                info: [
                    {
                        name: 'Name',
                        value: '',
                        pattern: /^[a-zA-Z ]{2,30}$/
                    },
                    {
                        name: 'Phone',
                        value: '',
                        pattern: /^[0-9]{7,14}$/
                    },
                    {
                        name: 'Email',
                        value: '',
                        pattern: /.+/,
                    },
                    {
                        name: 'Some Field 1',
                        value: '',
                        pattern: /.+/
                    },
                    {
                        name: 'Some Field 2',
                        value: '',
                        pattern: /.+/
                    }
                ],
            sendResult: false,
            }
        },
        methods: {
            onInput(i, e){
                let field = this.info[i];

                field.value = e.value;
                field.valid = field.pattern.test(field.value);
            },
            onSend(){
                this.sendResult = !this.sendResult;
            },
        },
        computed: {
            fieldsDone(){
                return this.info.reduce((total, field) => {
                    return total + (field.valid ? 1 : 0);
                }, 0);
            },
            formReady(){
                return this.fieldsDone === this.info.length ? true : false;
            },
            progressWidth(){
                return this.fieldsDone/this.info.length * 100;
            },
        },
        created(){
            let num = 0;
            this.info.forEach(field => {
                this.$set(field, 'valid', false);
                this.$set(field, 'id', num);
                num++;
            });
        }
    }
</script>
<style>
    .wrapper{
        padding: 15px;
    }
</style>
