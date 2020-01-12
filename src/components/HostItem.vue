<template>
      <div class="card-panel flex valign-wrapper">
            <div class="name"><b>{{ id+1 }}:</b> {{ host.url }}</div>
            <div class="location flex valign-wrapper">
                <div class="location-path">{{ host.path }}</div>
                <div class="location-copy">
                    <div class="waves-effect waves-teal btn-flat" @click="copyPath">
                        <i class="material-icons copy">file_copy</i>
                        </div>
                </div>
            </div>
            <a class="waves-effect waves-light btn red" @click="deleteItem">
                <i class="material-icons">delete</i>
            </a>
      </div>
</template>

<script>
export default {
    props: ['host', "id"],
    methods: {
        deleteItem() {
            this.$emit('app:delete', this.host.url);
        },
        async copyPath() {
          await navigator.clipboard.writeText(this.host.path) ;
        }
    }
}
</script>

<style>
    .list {
        margin-top: 35px;
    }
    .flex {
        display: flex;
        justify-content: space-between;
    }


</style>