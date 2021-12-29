<template>
    <div>
        <nav class="navbar">
            <ul id="ul-navigation" class="nav-ul" data-visible="false">
                <h1>Eurosong voting app</h1>
                <li class="nav-li" ><a @click="goToPage('home')">Home</a></li>
                <li class="nav-li"><a @click="goToPage('game')">Vote</a></li>
                <li class="nav-li"><a class="active" @click="goToPage('ranking')">Ranking</a></li>
            </ul>
            <button class="mobile-toggle" aria-controls="ul-navigation" aria-expanded="false">
                <span class="sr-only">Menu</span>
            </button>
        </nav>
        <div style="width:100vw; height:15vh;"></div>
        <div class="options">
            <div v-for="(song, index) in songs" :key="index" class="option" v-bind:class="{active: index==activeInd}" @click="changeActive(option, $event)">
                <RankingItem 
                    :songtitle="song.title" 
                    :points="song.point"
                    :artist="song.artist.name"
                    :index="index"
                    :songlink="song.spotify"
                >
                </RankingItem>
            </div>
        </div>

    </div>
</template>
<script>
    import RankingItem from "../components/RankingItem.vue"
    export default {
        name: "Rankingpage",
        components: {
            RankingItem
        },
        data() {
            return {
                points: [],   // { songID: points}      
                songs: [],
                activeInd: 0      
            }
        },
        mounted() {
            this.fetchSongs();

        },
        methods: {
            changeActive(option, e) {
                var target = e.currentTarget;
                //console.log(target);
                var previous = document.querySelector("div.active");
                //console.log(previous)
                previous.classList.remove("active")
                //console.log(previous)
                target.classList.add('active');
                //console.log(target);
            },
            // navigation method
            goToPage(page) {
                this.$emit("change-page", page);
            },
            // data methods
            fetchPoints() {
                // Get all songs
                const url = "http://webservies.be/eurosong/Votes";
                fetch(url)
                    .then((response) => {
                        return response.json();
                    })
                    .then((votes) => {
                        this.cleanData(votes);
                    });
            },
            cleanData(votes) {
                votes.forEach(vote => {
                    if (vote.songID in this.points) {
                        //console.log(vote.songID);
                        this.points[vote.songID] += vote.points;
                    }
                });
                let points_copy = this.points
                var items = Object.keys(points_copy).map(function(key) {
                    //console.log(key)
                    return [key, points_copy[key]];
                });
                

                // Sort the array based on the second element
                items.sort(function(first, second) {
                    return second[1] - first[1];
                });
                this.points = items;
                //console.log(this.songs)
                this.songs.map((song) => {
                    // find the artist in an array
                    const point = this.points.find((point) => point[0] == song.id);
                    song.point = point[1];
                    // return the new object
                    return song;
                });
                this.songs.sort(function(a, b) {
                    //console.log(a)
                    return b.point - a.point;
                });
            },
            fetchSongs() {
                // Get all songs
                const url = "http://webservies.be/eurosong/Songs";
                fetch(url)
                    .then((response) => {
                        return response.json();
                    })
                    .then((songs) => {
                        this.fetchArtists(songs);
                    })
                    .then(() => {
                        this.fetchPoints();
                    })
            },
            fetchArtists(songs) {
                // Get all artist
                const url = "http://webservies.be/eurosong/Artists";
                fetch(url)
                    .then((response) => {
                        // response is text, so convert to json
                        return response.json();
                    })
                    .then((artists) => {
                        // loop over array songs with forEach method
                        songs.map((song) => {
                            // find the artist in an array
                            const artist = artists.find((artist) => artist.id == song.artist);
                            // replace the id by the artist object
                            song.artist = artist;
                            // return the new object
                            return song;
                        });
                        // change data of songs, so everything will get rerenderd;
                        this.songs = songs;

                        this.songs.forEach(song => {
                            //let s_id = song.id;
                            this.points[song.id] = 0
                        });
                    });
                    
            },
            getSongName(songID) {
                this.songs.forEach(song => {
                    //console.log(song.title)
                    if (song.id == songID)  return song.title
                });
            },
            goToPage(page) {
                this.$emit("change-page", page);
            }
        }
    }
</script>