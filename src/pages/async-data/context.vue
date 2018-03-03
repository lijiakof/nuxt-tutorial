<template>
    <div>
        <h1>context</h1>
        {{userAgent}}<br>
        {{cookie}}
    </div>
</template>

<script>

export default {
    asyncData ({ req }) { 
        return {
            userAgent: (req ? req.headers['user-agent'] : (typeof navigator !== 'undefined' ? navigator.userAgent : 'No user agent (generated)')),
            cookie: req ? req.headers['cookie'] : ''
        };
    },
    head: {
        title: 'context'
    }
}
</script>
