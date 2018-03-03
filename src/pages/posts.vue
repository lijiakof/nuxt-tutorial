<template>
    <div>
        <h1>Hello Blog</h1>
    </div>
</template>
<script>
export default {
    layout: 'blog',
    head () {
        return {
        title: 'hello',
        meta: [
            { hid: 'description', name: 'description', content: 'My custom description' }
        ]
        }
    }
}
</script>