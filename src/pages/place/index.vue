<template>
    <div>
        <h1>place</h1>
        {{place}}
    </div>
</template>

<script>
import axios from 'axios'

export default {
    asyncData ({ params }) {
        return axios.get(`http://api.baichanghui.com/productservice/places/search?city=%E5%8C%97%E4%BA%AC`)
            .then((res) => {
            return {place: res.data}
        })
    },
    head: {
        title: 'Place'
    }
}
</script>
