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
        <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-5 px-10 relative">
            <div
                class="group cursor-pointer w-full h-96 rounded-2xl relative overflow-hidden"
                v-for="(movie, index) in movies"
                :key="index"
                :style="{ 
                    backgroundImage: `url(https://image.tmdb.org/t/p/w500/${movie.backdrop_path})`, 
                    backgroundPosition: 'center', 
                    backgroundSize: 'cover', 
                    backgroundRepeat: 'no-repeat' 
                }"
                >
                <p class="text-gray-200 font-bold text-lg flex items-center justify-between px-10 py-5">
                    <span class="text-lg text-white font-bold uppercase">{{ movie.title }}</span>
                    <span class="bg-blue-600 px-2 rounded-md text-white">{{ movie.vote_count }} votes</span>
                </p>

                <!-- Overview with smooth left-to-right animation -->
                <div 
                    class="absolute left-0 bottom-0 w-full bg-black bg-opacity-50 p-4 text-center text-white 
                            transform -translate-x-full group-hover:translate-x-0 transition-transform duration-500 ease-out"
                >
                    <p>{{ movie.overview }}</p>
                </div>
            </div>
        </div>
    </div>
</template>