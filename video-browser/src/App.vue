<template>
    <div class="container">
        <!-- v-on:termChange="onTermChange" -->
        <SearchBar @termChange="onTermChange"></SearchBar>
        <!-- v-bind:videos="videos" -->
        <div class="row">
            <VideoDetail :video="selectedVideo" />
            <VideoList
                :videos="videos"
                @videoSelect="onVideoSelect"
            ></VideoList>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';
const API_KEY = 'AIzaSyCdr7kK-RH5z17BWnmM26RJv1J0TyY6bFc';

export default {
    name: 'App',
    components: {
        SearchBar,
        VideoList,
        VideoDetail
    },
    data() {
        return {
            videos: [],
            selectedVideo: null
        }
    },
    methods: {
        onVideoSelect(video) {
            this.selectedVideo = video;
        },
        onTermChange(searchTerm) {
            axios.get('https://www.googleapis.com/youtube/v3/search', {
                params: {
                    key: API_KEY,
                    type: 'video',
                    part: 'snippet',
                    q: searchTerm
                }   
            }).then(response => {
                this.videos = response.data.items;
            })
                .catch(err => console.log(err));
        }
    }
}
</script>