<template>
    <div style="height: auto;
    display: flex;
    flex-direction: column;
    align-items: center;"
>
        <nav class="navbar">
            <ul id="ul-navigation" class="nav-ul" data-visible="false">
                <h1>Eurosong voting app</h1>
                <li class="nav-li" ><a @click="goToPage('home')">Home</a></li>
                <li class="nav-li"><a class="active" @click="goToPage('game')">Vote</a></li>
                <li class="nav-li"><a @click="goToPage('ranking')">Ranking</a></li>
            </ul>
            <button class="mobile-toggle" aria-controls="ul-navigation" aria-expanded="false">
                <span class="sr-only">Menu</span>
            </button>
        </nav>
        <div style="width:100vw; height:15vh;"></div>
        <div style="width:80vw; height: 10vh; display:flex; justify-content: space-around">
            <div style="width:75%">
                <Carousel
                    :items="songs"
                    :activeIndex="activeSongIndex"

                    @change-index="changeActiveSongIndex"
                />
            </div>
            <div style="width:25%">
                <div v-for="(vote, index) in votes" :key="index" class="votinglist" :class="{ nonedisplay: vote.isVoted }">
                    <div @click="addVote(vote.points)" class="votinglistitem">
                        <div class="line"></div>
                        <p class="points">Add {{ vote.points }} points</p>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    // components
    import Carousel from "../components/Carousel.vue";
    // export
    export default {
        name: "Gamepage",
        components: {
            Carousel
        },
        data() {
            return {
                songs: [],
                activeSongIndex: 0,
                votes: [
                    {
                        points: 1,
                        isVoted: false
                    },
                    {
                        points: 2,
                        isVoted: false
                    },
                    {
                        points: 4,
                        isVoted: false
                    },
                    {
                        points: 8,
                        isVoted: false
                    },
                    {
                        points: 12,
                        isVoted: false
                    }
                ]
            }
        },
        mounted() {
            this.fetchSongs();
        },
        methods: {
            // navigation method
            goToPage(page) {
                this.$emit("change-page", page);
            },
            // data methods
            fetchSongs() {
                // Get all songs
                const url = "http://webservies.be/eurosong/Songs";
                fetch(url)
                    .then((response) => {
                        return response.json();
                    })
                    .then((songs) => {
                        this.fetchArtists(songs);
                    });
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
                    });
            },
            postVote(points) {
                const songId = this.songs[this.activeSongIndex].id;
                const url = "http://webservies.be/eurosong/Votes";
                fetch(url, {
                    method: "POST",
                    headers: {
                        'Accept': 'application/json, text/plain',
                        'Content-Type': 'application/json;charset=UTF-8'
                    },
                    body: JSON.stringify({
                        songID: songId,
                        points: points,
                    })
                }).then((response) => {
                    return response.json();
                }).then((result) => {
                    console.log(result);
                })
            },
            // logic methods
            changeActiveSongIndex(index) {
                this.activeSongIndex = index;
            },
            addVote(points) {
                let votes = this.votes;
                // create new array when points is equal to given points, change isVoted state so it dissepears
                votes.map((vote) => {
                    if (vote.points == points) {
                        vote.isVoted = true;
                    }
                    return vote;
                });
                // post it to the api
                this.postVote(points);
                // check if all votes isVoted are set to true OR there are no false votes
                let activeVotes = votes.filter((vote) => vote.isVoted == true);
                // if everything is voted, redirect to ranking
                if (activeVotes.length == votes.length) {
                    this.goToPage("ranking");
                }
            }
        }
    }
</script>