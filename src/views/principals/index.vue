<script setup>

    //import ref and onMounted
    import axios from 'axios';
    import { ref, onMounted } from 'vue';

    //import api
    import api from '../../api';

    //define state
    const posts = ref([]);

    //method fetchDataPosts
    const fetchDataPosts = async () => {
        
        const response = await axios.get('https://api.ipify.org?format=json')
        // extract IP address from response data
        const ipAddress = response.data.ip
        const options = {
            headers: {
                'Authorization': 'eyJpdiI6Ikc0RHdiOUF0TU9IQWpoVVR3SVFNU0E9PSIsInZhbHVlIjoiN2pEd3ZObmZTYStYdVhzL1NFUGlldz09IiwibWFjIjoiMTVkMGQyYTQxYWRmM2IzZWY4OWVmZTUyOGUwNGFmY2E1MzAwZTBlYTIwNjI5ODRkODVhYTQyZjIyYTgwMWJkZSIsInRhZyI6IiJ9YspAJbi4jljFFRy',
                'X-Forwarded-For': await ipAddress

            },
        }
        //fetch data 
        await api.get('/api/v1/principals', options)

        .then(response => {

            //set response data to state "posts"
            posts.value = response.data.data.data

        });
    }

    //run hook "onMounted"
    onMounted(() => {

        //call method "fetchDataPosts"
        fetchDataPosts();
    });

</script>

<template>
    <div class="container mt-5 mb-5">
        <div class="row">
            <div class="col-md-12">
                <!-- <router-link :to="{ name: 'posts.create' }" class="btn btn-md btn-success rounded shadow border-0 mb-3">ADD NEW POST</router-link> -->
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <table class="table table-bordered">
                            <thead class="bg-dark text-white">
                                <tr>
                                    <th scope="col">ID</th>
                                    <th scope="col">Name</th>
                                    <th scope="col">Address</th>
                                    <!-- <th scope="col" style="width:15%">Actions</th> -->
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-if="posts.length == 0">
                                    <td colspan="4" class="text-center">
                                        <div class="alert alert-danger mb-0">
                                            Data Belum Tersedia!
                                        </div>
                                    </td>
                                </tr>
                                <tr v-else v-for="(post, index) in posts" :key="index">
                                    <!-- <td class="text-center">
                                        <img :src="post.image" width="200" class="rounded-3"/>
                                    </td> -->
                                    <td>{{ post.id }}</td>
                                    <td>{{ post.name }}</td>
                                    <td>{{ post.address }}</td>
                                    <!-- <td class="text-center">
                                        <router-link :to="{ name: 'posts.edit', params:{id: post.id} }" class="btn btn-sm btn-primary rounded-sm shadow border-0 me-2">EDIT</router-link>
                                        <button class="btn btn-sm btn-danger rounded-sm shadow border-0">DELETE</button>
                                    </td> -->
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>