<template>
  <div>
      <div style="height:300px; width:100%" class="content"> <iframe :src="songlink" width="100%" height="400" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe></div>
        <div class="shadow"></div>
        <div class="label">
            <div class="icon">
                <p>{{index+1}}</p>
            </div>
            <div class="info">
                <div class="main">{{songtitle}}</div>
                <div class="sub">{{artist}}</div>
                <div class="sub">{{points}} points</div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "RankingItem",
        props: [
            'points',
            'artist',
            'songtitle',
            'index',
            'songlink'
        ]
    }
</script>