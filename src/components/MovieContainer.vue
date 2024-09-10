<script setup>
import {ref, onMounted} from "vue";
import axios from "axios";

const movies = ref([]);

const mtdbKey = import.meta.env.VITE_MTDB_API_KEY;

async function getMovies() {
    try {
        const response = await axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=${mtdbKey}`);
        movies.value = response.data.results;
        console.log(movies.value);
    } catch (error) {
        console.error('Error fetching movies:', error);
    }
}

async function handleMovieDetails(id) {
    try {
        const response = await axios.get(`https://api.themoviedb.org/3/movie/${id}?api_key=${mtdbKey}`);
    }catch(error){
        console.error('Error fetching movies:', error);
    }
}


onMounted(() => {
    getMovies();
})


</script>

<template>
    <div class="w-full bg-gray-900">
        <h3 class="text-center text-3xl text-white font-bold pt-5 mb-8">My Movie App</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-5 px-10">
            <div class="group cursor-pointer w-full h-96 rounded-2xl relative" v-for="(movie, index) in movies" :key="index">
                <img class="w-full h-full object-cover object-center rounded-md" :src="'https://image.tmdb.org/t/p/w500/' + `${movie.backdrop_path}`" alt="Movie Backdrop" />
                <div class="hidden group-hover:block absolute text-center inset-0">
                    <p class="text-gray-200 font-bold text-lg mb-3">{{ movie.title}}</p>
                    <button @click="handleMovieDetails(movie.id)" class="bg-blue-600 px-2 py-1 rounded text-white font-bold ">Details</button>
                </div>
            </div>
        </div>
    </div>
</template>