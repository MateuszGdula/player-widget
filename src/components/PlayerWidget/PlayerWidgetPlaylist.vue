<template>
  <div class="playlist" :class="{ open }" ref="playlist">
    <div class="playlist__top">
      <h5>Playlist</h5>
      <div class="playlist__back-arrow">
        <player-widget-btn
          :icon="backIcon"
          :bgColor="'transparent'"
          @click.native="$emit('togglePlaylist', '')"
        />
      </div>
    </div>
    <div @scroll="setScroll" class="playlist__list-wrapp" ref="listWrapp">
      <ul ref="list">
        <li
          class="playlist__item"
          v-for="(track, i) in tracks"
          :key="i"
          @click.self="
            $event => $emit('chooseTrack', { event: $event, index: i })
          "
        >
          <div class="playlist__texts">
            <p>{{ i }} {{ track.time }} | {{ track.artist }}</p>
            <h6>{{ track.title }}</h6>
          </div>
          <div class="playlist__icons">
            <player-widget-btn
              :icon="shareIcon"
              :bgColor="'transparent'"
              :size="'22px'"
            />
            <player-widget-btn
              :icon="favoriteIcon"
              :bgColor="'transparent'"
              :size="'22px'"
            />
          </div>
        </li>
      </ul>
      <div
        class="playlist__scrollbar"
        ref="scroll"
        :style="{
          transform: 'translateY(' + scrollPosition + 'px)',
          display: displayScroll
        }"
      ></div>
    </div>
  </div>
</template>

<script>
import PlayerWidgetBtn from "@/components/PlayerWidget/PlayerWidgetBtn.vue";
import backIcon from "@/assets/img/back-arrow-icon.svg";
import shareIcon from "@/assets/img/share-icon.svg";
import favoriteIcon from "@/assets/img/favorite-icon.svg";

export default {
  name: "PlayerWidgetPlaylist",
  components: {
    PlayerWidgetBtn
  },
  props: {
    tracks: Array,
    open: Boolean
  },
  data() {
    return {
      scrollPosition: 0,
      displayScroll: "none",
      backIcon,
      shareIcon,
      favoriteIcon
    };
  },
  methods: {
    setScroll() {
      let scrollPercent =
        this.$refs.listWrapp.scrollTop /
        (this.$refs.list.offsetHeight - this.$refs.listWrapp.offsetHeight);
      this.scrollPosition = Math.max(
        0,
        scrollPercent * this.$refs.listWrapp.offsetHeight -
          this.$refs.scroll.offsetHeight
      );
    }
  },
  mounted() {
    this.displayScroll =
      this.$refs.list.offsetHeight > this.$refs.listWrapp.offsetHeight
        ? "block"
        : "none";
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h5 {
  font-size: 18px;
  color: #60558f;
  font-family: "Rambla";
  font-weight: 700;
  text-align: center;
}
p {
  margin: 0 0 4px 0;
  font-size: 12px;
  color: #78747f;
}
.playlist {
  width: 100%;
  height: 480px;
  background-color: #f3f4f8;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 200;
  transform: translateX(100%);
  transition: transform 0.3s ease-out;
  &::after {
    content: "";
    width: 285px;
    height: 50px;
    background: linear-gradient(
      to bottom,
      rgba(243, 244, 248, 0) 0%,
      rgba(243, 244, 248, 1) 100%
    );
    position: absolute;
    bottom: 45px;
    left: 25px;
    pointer-events: none;
  }
  &.open {
    transform: translateX(0);
  }
  &__top {
    height: 65px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
  }
  &__back-arrow {
    position: absolute;
    left: 20px;
    top: 50%;
    transform: translateY(-50%);
  }
  &__list-wrapp {
    height: 370px;
    padding: 0 50px 0 25px;
    position: relative;
    overflow-y: auto;
    scrollbar-width: none;
    &::-webkit-scrollbar {
      display: none;
    }
  }
  &__texts {
    pointer-events: none;
  }
  &__scrollbar {
    width: 7px;
    height: 70px;
    background-color: #2a224d;
    border-radius: 3px;
    position: fixed;
    top: 65px;
    right: 20px;
    opacity: 0.1;
  }
  &__item {
    height: 75px;
    border-bottom: 1px solid #d3d5de;
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: background-color 0.3s ease-out;
    cursor: pointer;
    &:hover,
    &:focus {
      background-color: #e9e9e9;
    }
  }
  &__icons {
    display: flex;
  }
}
</style>
