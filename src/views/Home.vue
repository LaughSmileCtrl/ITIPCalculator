<template>
    <section
        class="text-gray-600 body-font bg-center bg-no-repeat bg-cover min-h-screen"
        style="background-image: url('img/bg.png')"
    >
        <NavBar />
        <div
            class="
                container
                mx-auto
                flex
                px-2
                py-8
                md:flex-row
                flex-col
                items-center
            "
        >
            <div class="lg:max-w-lg lg:w-full md:w-1/2 w-5/6 mb-10 md:mb-0">
                <img
                    class="object-cover object-center rounded"
                    alt="hero"
                    src="../assets/welcome.png"
                />
            </div>
            <div
                class="
                    lg:flex-grow
                    md:w-1/2
                    lg:pl-24
                    md:pl-16
                    flex flex-col
                    md:items-start md:text-left
                    items-center
                    text-center
                    px-10
                "
            >
                <h1 class="font-bold title-font text-4xl mb-16 text-purple-700 dark:text-purple-50">
                    Selamat datang di Aplikasi Penghitung Indeks Prestasi per
                    Semester
                </h1>
                <p class="mb-8 leading-relaxed"></p>
                <div class="flex justify-center">
                    <router-link class="btn btn-primary rounded-full px-8" to="/calculate">
                        Lanjut <i class="fas fa-arrow-right m-2"></i>
                    </router-link>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import NavBar from "@/components/NavBar.vue"

export default {
    name: "Home",
    components: {
        NavBar,
    },
};
</script>
