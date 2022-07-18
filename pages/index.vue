<template>
<div>
    <v-container>
        <v-row justify="center" align="center">
            <v-col align="center">
                <h3>THREE JS TUTORIAL</h3>
                https://www.youtube.com/watch?v=xJAfLdUgdc4&list=PLjcjAqAnHd1EIxV4FSZIiJZvsdrBc1Xho
                <v-btn @click="$router.push('/create-scene')" class="mt-5">Start Learning</v-btn>
            </v-col>
        </v-row>
    </v-container>
</div>
</template>

<script>
export default {
    name: 'IndexPage'
}
</script>
