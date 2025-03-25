<script setup>
import { ref, onMounted, nextTick } from "vue";
import Hls from "hls.js";

const channels = ref([
  {
    name: "IPL 2025 - 1",
    link: "http://kst.moonplex.net:8080/CH2/tracks-v1a1/mono.m3u8",
    logo: "https://upload.wikimedia.org/wikipedia/en/1/18/2025_IPL_logo.png",
  },
  {
    name: "IPL 2025 - 2",
    link: "https://cdn.hoichoi24.com/LIVE/video.m3u8",
    logo: "https://upload.wikimedia.org/wikipedia/en/1/18/2025_IPL_logo.png",
  },
  {
    name: "T Sports 1",
    link: "https://live.tsports.com/mobile_hls/tsports_live_1/playlist.m3u8",
    logo: "https://raw.githubusercontent.com/subirkumarpaul/Logo/main/T%20Sports.png",
  },
  {
    name: "T Sports 2",
    link: "https://live.tsports.com/mobile_hls/tsports_live_2/playlist.m3u8",
    logo: "https://raw.githubusercontent.com/subirkumarpaul/Logo/main/T%20Sports.png",
  },
  {
    name: "Ary Sports",
    link: "http://125.209.88.166:45793/BRN/ArySports.stream/chunklist.m3u8",
    logo: "https://upload.wikimedia.org/wikipedia/en/1/18/2025_IPL_logo.png",
  },
]);

const player = ref(null);
const currentStream = ref(channels.value[0].link);

const playStream = (link) => {
  currentStream.value = link;
  nextTick(() => {
    if (Hls.isSupported()) {
      let hls = new Hls();
      hls.loadSource(link);
      hls.attachMedia(player.value);
      hls.on(Hls.Events.MANIFEST_PARSED, () => {
        player.value.play();
      });
    } else if (player.value.canPlayType("application/vnd.apple.mpegurl")) {
      player.value.src = link;
      player.value.play();
    } else {
      alert("Your browser does not support HLS streaming.");
    }
  });
};

onMounted(() => {
  playStream(currentStream.value);
});
</script>

<template>
  <div class="container">
    <div class="desktop-ads left-ad">Ad Banner</div>
    <div class="content">
      <div class="stream-title">LIVE Streaming</div> <!-- New Title -->
      <div class="mobile-ad">Ad Banner (Top)</div>
      <video ref="player" controls></video>
      <div class="mobile-ad">Ad Banner (Bottom)</div>
      <div class="channels">
        <div
          v-for="channel in channels"
          :key="channel.name"
          class="channel"
          @click="playStream(channel.link)"
        >
          <img :src="channel.logo" :alt="channel.name" />
          <p>{{ channel.name }}</p>
        </div>
      </div>
    </div>
    <div class="desktop-ads right-ad">Ad Banner</div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: Arial, sans-serif;
  text-align: center;
  background: #f5f5f5;
}

.container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
  gap: 10px;
}

/* Desktop Ads */
.desktop-ads {
  width: 160px;
  min-height: 600px;
  background: #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: bold;
  color: #333;
}
.left-ad {
  margin-right: 10px;
}
.right-ad {
  margin-left: 10px;
}

.content {
  max-width: 700px;
  width: 100%;
  position: relative;
}

/* Mobile Ads */
.mobile-ad {
  display: none;
  width: 100%;
  max-height: 80px;
  background: #ddd;
  text-align: center;
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin: 10px 0;
  padding: 10px;
}

video {
  width: 100%;
  height: 400px;
  background: black;
  border-radius: 10px;
}

/* Stylish Streaming Title */
.stream-title {
  font-size: 36px;
  font-weight: bold;
  background: linear-gradient(to right, #ff7e5f, #feb47b); /* Gradient */
  -webkit-background-clip: text;
  color: transparent;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
  /* margin: 5px 0; */
  animation: gradientText 3s ease-in-out infinite; /* Animation */
  text-align: center;
}

/* Channel Design */
.channels {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
  margin-top: 10px;
}

.channel {
  width: 90px;
  height: 110px;
  cursor: pointer;
  text-align: center;
  background: #fff;
  padding: 8px;
  border-radius: 8px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.channel:hover {
  transform: scale(1.05);
}

.channel img {
  width: 60px;
  height: 60px;
  object-fit: contain;
  border-radius: 6px;
}

.channel p {
  font-size: 12px;
  margin-top: 5px;
  font-weight: bold;
}

/* Mobile View: Hide Side Ads, Show Top & Bottom Ads */
@media (max-width: 1024px) {
  .container {
    flex-direction: column;
    align-items: center;
  }
  .desktop-ads {
    display: none;
  }
  .mobile-ad {
    display: block;
  }
}

/* Gradient Text Animation */
@keyframes gradientText {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>
