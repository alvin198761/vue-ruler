<template>
  <div>
    <h1>仅限于移动端</h1>
    体重
    <DLRuler :value="50" :min="30" :max="300" :onChange="changeWeight"></DLRuler>
    身高
    <DLRuler :value="60" :min="10" :max="100"></DLRuler>
    测试
    <DLRuler :value="35" :min="10" :max="120"></DLRuler>
  </div>

</template>
<script>
    import DLRuler from './components/DLRuler.vue';

    export default {
        components: {DLRuler},
        data: function () {
            return {}
        },
        computed: {},
        created: function () {

        },
        methods: {
            changeWeight(val) {
                console.log(val);
            }
        }
    }
</script>
<style>

</style>
