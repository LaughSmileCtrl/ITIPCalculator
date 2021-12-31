<template>
    <section
        class="text-gray-600 body-font bg-center bg-no-repeat bg-cover min-h-screen pb-8"
        style="background-image: url('img/bg.png')"
    >
        <NavBar />

        <div class="md:px-12 lg:px-56">

            <div class="grid grid-cols-1 content-center mx-5 ">
                <h2 class="text-2xl mx-4 text-purple-800 dark:text-white">
                    Form Nilai
                </h2>

                <div class="form-control mx-4 mt-4">
                    <label class="label" for="semester">
                        <span class="label-text dark:text-white">Pilih Semester</span>
                    </label> 
                    <select id="semester" class="select select-sm select-bordered select-primary w-24" @change="getSubjects($event)">
                        <option disabled="disabled" selected="selected">Pilih Semester</option> 
                        <option>1</option> 
                        <option>2</option> 
                        <option>3</option> 
                        <option>4</option> 
                        <option>5</option> 
                        <option>6</option> 
                    </select>
                </div>

                <div class="rounded-lg overflow-x-auto m-5">
                    <table v-if="renderState" class="p-1 text-sm text-purple-800 dark:text-white bg-purple-50 dark:bg-purple-700 w-full">
                        <thead class="rounded-none sticky top-0 z-20 bg-purple-50 dark:bg-purple-700 p-3">
                            <tr>
                                <th></th> 
                                <th class="py-3">Mata Kuliah</th> 
                                <th>Jumlah SKS</th> 
                                <th class="">Nilai</th>
                            </tr>
                        </thead> 
                        <tbody class="text-purple-600 dark:text-purple-100">
                            <tr v-for="(subject, i) in subjects" :key="i" class="border-y border-white">
                                <th class="py-3 px-2"> {{ i+1 }}</th> 
                                <td class="py-3 px-2">
                                    <div class="font-bold">
                                        {{ subject.name }}
                                    </div>
                                    <p class="text-xs text-red-500">
                                        {{ errors[subject.subid] }}
                                    </p>
                                </td> 
                                <td class="text-center">{{ subject.sks }}</td> 
                                <td class="sticky right-0 py-1 px-2 text-center">
                                    <select :id="subject.subid" v-model="scores[subject.subid]" class="select select-sm select-bordered select-primary w-16 max-w-xs text-purple-800">
                                        <option disabled="disabled" selected="selected">Pilih Nilai</option> 
                                        <option>A</option> 
                                        <option>AB</option> 
                                        <option>B</option>
                                        <option>BC</option>
                                        <option>C</option>
                                        <option>D</option>
                                        <option>E</option>
                                    </select>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <div class="justify-self-end mx-4">
                    <button class=" btn btn-primary rounded-full px-8 md:w-56" @click="showResult">
                        Hitung
                    </button>
                </div>
            </div>
        </div>
    </section>

    <div v-if="showOverlay" class="fixed top-0 bottom-0 right-0 left-0 bg-purple-200 z-50 opacity-40"></div>
</template>

<script>
import NavBar from '@/components/NavBar.vue'
import axios from 'axios'

export default {
    data() {
        return {
            renderState: false,
            subjects: [],
            scores: {
                semester: this.semester,
            },
            errors: {},
            url: 'https://itipcalculatormang.000webhostapp.com',
            showOverlay: false,
        }
    },
    components: {
        NavBar,
    },
    methods: {
        showResult() {
            var isFull = true;
            this.subjects.forEach((subject) => {
                isFull &= (this.scores[subject.subid] != null);

                if (this.scores[subject.subid] == null) {
                    this.errors[subject.subid] = 'nilai '+subject.name+' tidak boleh kosong'
                } else {
                    delete this.errors[subject.subid];
                }
            })

            if (isFull) {
                axios.post(this.url + '/api/students', this.scores, {
                    onUploadProgress: () => {
                       this.showOverlay = true;
                    },


                    onDownloadProgress: () => {
                        this.showOverlay = true;                       
                    },
                })
                .then(response => {
                    this.$swal('IP anda di Semester ini adalah ' + response.data.result, '' , 'success');
                })
                .catch()
                .then(() => {
                    this.showOverlay = false;
                });

            }
        },
        getSubjects(event) {
            this.scores.semester = event.target.value;
            axios.get(this.url + '/api/semesters/'+ this.scores.semester, {
                onUploadProgress: () => {
                    this.showOverlay = true;
                    
                },


                onDownloadProgress: () => {
                    this.showOverlay = true;                       
                },
            })
            .then((response) => {
                this.subjects = response.data.data;
                this.renderState = true;
                this.subjects.forEach(subject => {
                    this.scores[subject.subid] = null;
                })
            })
            .catch()
            .then(() => {
                this.showOverlay = false;
            });
    }
    }
}
</script>
