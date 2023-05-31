<template>
        <Link :href="route('portfolioCryptos.show')" as="button" class="logo">ПНКИ</Link>
        <p class="ml-2 text-xs text-blue-900 mt-3">Платформа начинающего крипто инвестора</p>
</template>

<script>

import { Link } from '@inertiajs/inertia-vue3'

export default {
    components: {
        Link,
    },
    setup() {

    },
}
</script>
