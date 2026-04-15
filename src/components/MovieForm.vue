<template>
    <div class="container mt-5">
        <h1>Add a Movie</h1>
        <form id="movieForm" @submit.prevent="saveMovie" enctype="multipart/form-data">
        <div class="mb-3">
            <label for="title" class="form-label">Title</label>
            <input type="text" name="title" class="form-control">
        </div>
        <div class="mb-3">
            <label for="description" class="form-label">Description</label>
            <textarea class="form-control" name="description"></textarea>
        </div>
        <div class="mb-3">
            <label for="poster" class="form-label">Poster</label>
            <input type="file" class="form-control" name="poster">
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
let csrf_token = ref('');

    function getCSRFToken() {
        fetch('/api/v1/csrf-token')
            .then(response => response.json())
            .then(data => {
                console.log(data);
                csrf_token.value = data.csrf_token;
            })
    }

onMounted(() => {
    getCSRFToken();
});

function saveMovie() {
    let movieForm = document.getElementById('movieForm');
    let form_data = new FormData(movieForm);
    fetch("/api/v1/movies", {
        method: 'POST',
        body: form_data,
        headers: {
            'X-CSRFToken': csrf_token.value
        }
    })
        .then(function (response) {
            return response.json();
        })
        .then(function (data) {
            console.log(data);
        })
        .catch(function (error) {
            console.log(error);
        });
}
</script>