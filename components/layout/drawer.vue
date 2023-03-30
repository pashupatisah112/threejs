<template>
<div>
    <v-navigation-drawer v-model="drawer" min-width="200" app permanent>
        <v-list dense>
            <v-list-item v-for="(item,i) in content" :key="i" :to="item.link">

                <v-list-item-content>
                    <v-list-item-title>{{ item.title }}</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
        </v-list>
    </v-navigation-drawer>
</div>
</template>

<script>
export default {
    data() {
        return {
            drawer: true,
            content:[
                {title:'Installation',link:'/'},
                {title:'Creating scene',link:'/create-scene'},
                {title:'Drawing line',link:'/drawing-line'},
                {title:'Creating Text',link:'/creating-text'},
                {title:'Basic Details',link:'/basic-detail'},
                {title:'Loading 3d Models',link:'/loading-3d-models'},
                {title:'Object Control GUI',link:'/gui-control'},
                {title:'Lighting',link:'/lighting'},
                {title:'Fog',link:'/fog'},
                {title:'Background',link:'/background'},
                {title:'Selecting Object',link:'/selecting-object'},
                {title:'Responsive',link:'/Responsive'},

            ]

        }
    },

}
</script>
