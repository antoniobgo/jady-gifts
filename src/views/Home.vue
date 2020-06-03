<template lang="pug">
v-row(justify="center")
  v-card(outlined max-width="400")
    v-list(
      v-if="giftList"
      threeLine
      )
      template(v-for="(gift,index) in giftList")
        v-list-item.my-1(:key="index")
          v-list-item-avatar
            v-img(:src="getImgLink(gift)")
          v-list-item-content
            v-list-item-title {{ gift.name }}
            v-list-item-subtitle.my-1(v-if="gift.priority") Prioridade alta
            v-list-item-subtitle.my-1(v-if="!gift.avaiable") Esse produto já foi comprado por alguém
            v-list-item-subtitle.my-1(v-else) Produto disponivel para compra
        v-row.mb-2(dense justify="center")
          v-btn(
            v-if="gift.avaiable"
            @click="setAsBought(gift)"
            :loading="loading"
            )
            | Marcar como comprado
          v-btn(
            v-else
            @click="setAsAvaiable(gift)"
            :loading="loading"
            )
            | Marcar como disponível

        v-divider
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      giftList: undefined,
      loading: false,
    };
  },
  mounted() {
    axios
      .get("https://5ed52f3e8769250016e6338a.mockapi.io/presentes")
      .then((response) => {
        this.giftList = response.data;
      });
  },
  computed: {},
  methods: {
    getImgLink(gift) {
      return gift.imgLink;
    },
    setAsBought(gift) {
      gift.avaiable = false;
      this.loading = true;
      axios
        .put(
          "https://5ed52f3e8769250016e6338a.mockapi.io/presentes/" + gift.id,
          gift
        )
        .then(() => {
          this.loading = false;
        });
    },
    setAsAvaiable(gift) {
      gift.avaiable = true;
      this.loading = true;
      axios
        .put(
          "https://5ed52f3e8769250016e6338a.mockapi.io/presentes/" + gift.id,
          gift
        )
        .then(() => {
          this.loading = false;
        });
    },
  },
};
</script>
