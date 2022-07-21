<template>
    {{ time }}
</template>

<script>
var sf = require("shapefile");
import { toRefs, onMounted, reactive } from 'vue'
export default {

    setup() {

        const state = reactive({
            time: new Date()
        })
        onMounted(() => {
            setInterval(function () {
                state.time = new Date()
            }, 1000)
        })

        sf.open("t.shp")
            .then(source => source.read()
                .then(function log(result) {
                    if (result.done) return;
                    console.log(result.value);
                    return source.read().then(log);
                }))
            .catch(error => console.error(error.stack));
        return {
            ...toRefs(state)
        }
    }

}



</script>
<style lang="scss" scoped>
</style>