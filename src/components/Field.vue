<template>
    <div>
        <div class="form-group">
            <label>
                {{ name }}
                <v-icon v-if="activated" :name="fieldIcon" :class="filedClass"/>
            </label>
            <input type="text" class="form-control" :value="value" @input="onInput">
        </div>
    </div>
</template>
<script>
    import Icon from 'vue-awesome/components/Icon';

    export default {
        components: {
            'v-icon': Icon,
        },
        props: ['name', 'value', 'id', 'valid'],
        data(){
            return {
                activated: false,
            }
        },
        computed: {
            filedClass(){
                return this.valid ? 'text-success' : 'text-danger';
            },
            fieldIcon(){
                return this.valid ? 'check' : 'exclamation';
            }
        },
        methods: {
            onInput(e){
                this.activated = true;
                this.$emit('changevalue', { value: e.target.value.trim() });
            }
        }
    }
</script>
<style>
    .form-group {
        font-weight: bold;
    }
</style>
