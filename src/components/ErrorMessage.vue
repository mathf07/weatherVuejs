<template>
    <div v-if="error.length > 0" class="error">
        <div class="ui icon negative message">
            <i class="info circle icon"></i>
            <div class="content">
                <div class="header">{{error}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ErrorMessage",
    props: {
        error: String
    }
};
</script>


