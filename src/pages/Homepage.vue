<template>
    <div>
        <nav class="navbar">
            
            <ul id="ul-navigation" class="nav-ul" data-visible="false">
                <h1>Eurosong voting app</h1>
                <li class="nav-li" ><a class="active" @click="goToPage('home')">Home</a></li>
                <li class="nav-li"><a @click="goToPage('game')">Vote</a></li>
                <li class="nav-li"><a @click="goToPage('ranking')">Ranking</a></li>
            </ul>
            <button class="mobile-toggle" aria-controls="ul-navigation" aria-expanded="false">
                <span class="sr-only">Menu</span>
            </button>
        </nav>

        <div class="home_container">
            <div class="left">
                <div class="home_text">
                    <h1>Eurosong voting app</h1>
                    <h3>Vote for your favorite song</h3>
                    <div @click="goToPage('game')"><Homebutton :button_txt="'Let me vote'"></Homebutton></div>
                    <p>OR</p>
                    <h3>Look at how your favorite song is doing</h3>
                    <div @click="goToPage('ranking')"><Homebutton :button_txt="'Discover the ranking'"></Homebutton></div>

                </div>
            </div>
            <div class="right"><img src="../../src/assets/music.jpg" alt=""></div>
        </div>
    </div>
</template>

<script>
    import Homebutton from "../components/Homebutton.vue"      
    export default {
        components: { Homebutton },
        name: "Homepage",
        methods: {
            goToPage(page) {
                this.$emit("change-page", page);
            }
        }
    }
</script>