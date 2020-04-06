<template>
    <button>
        {{ numberOrChar }}
    </button>
</template>

<script>
export default {
    name: 'BtnNumber',
    props: ['numberOrChar'],
}
</script>
