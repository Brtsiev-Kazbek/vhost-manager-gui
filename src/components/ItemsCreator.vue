<template>
    <div class="creator creator-flex valign">
        <input type="text" placeholder="Create a host" v-model="hostName" title="Spaces are automatically replaced by dots">
        <div class="waves-effect waves-teal btn green add" @click="addHost">
        <i class="material-icons">add</i>
    </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            hostName: ''
        }
    },
    methods: {
        addHost() {
            this.$emit('app:create', this.hostName);
            this.hostName = ''
        }
    }
}
</script>

<style>
    .creator {
        margin-top: 25px;
    }
    .creator-flex {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .add {
        margin-left: 15px;
    }
</style>