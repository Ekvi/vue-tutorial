<template>
    <div>
        Type: {{type}}
        <div class="col-md-12">
            <Item
                v-for="(item, index) in items"
                key="index"
                v-bind:passed-item="item"
                v-bind:type="type">
            </Item>
        </div>
    </div>
</template>

<script>
    import Item from './Item.vue';

    export default {
        data() {
            return {
                type: this.$route.params.type,
                items: []
            }
        },
        watch: {
            '$route' : 'fetchItems'
        },
        methods: {
            fetchItems() {
                this.type = this.$route.params.type;

                this.items = [];
                let initial_ids = [1, 13, 14];

                for(let i in initial_ids) {
                    let id = initial_ids[i];
                    console.log('id', id);
                    fetch(`http://swapi.co/api/${this.type}/${id}`, {method: 'GET'})
                        .then(response => response.json())
                        .then(json => this.items.push(json))
                }
            }
        },
        created() {
            this.fetchItems();
        },
        components: {
            Item
        }
    }
</script>