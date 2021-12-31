<template>
    <div
        class="
            navbar
            p-5
        "
    >
        <div class="flex-1"></div>
        <div class="flex-none">
            <button class="btn btn-square btn-ghost bg-purple-500 dark:bg-purple-500" @click="changeTheme">
                <i v-if="!iconActive" class="fas fa-moon fa-lg text-purple-50"></i>
                <i v-else class="fas fa-sun fa-lg text-purple-50"></i>
            </button>
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            iconActive: true,
        }
    },
    methods: {
        changeTheme() {
            if (localStorage.theme != 'dark') {
                document.documentElement.classList.add('dark')
                localStorage.theme = 'dark'
                this.iconActive = true
            } else {
                document.documentElement.classList.remove('dark')
                localStorage.theme = 'light'
                this.iconActive = false
            }
        }
    }
}
</script>
