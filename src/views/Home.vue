<template lang="pug">
v-row(justify="center")
  v-card( max-width="380" outlined)
    v-list(
      v-if="giftList"
      threeLine
      )
      template(v-for="(gift,index) in giftList")
        v-row.mt-3(justify="center")
          v-img(:src="getImgLink(gift)" max-width="100" max-height="100")
        v-list-item.my-1(:key="index")
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
          v-dialog(v-model="dialog" max-width="290")
            v-card
              v-card-title Desculpe, ocorreu um erro
              v-divider
              v-card-actions
                v-spacer
                v-btn(@click="dialog=false") Ok

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
      dialog: false,
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
        .then((response) => {
          if (response.status != 200) {
            gift.avaiable = true;
            this.dialog = true;
          }
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
        .then((response) => {
          if (response.status != 200) {
            gift.avaiable = false;
            this.dialog = true;
          }
          this.loading = false;
        });
    },
  },
};
</script>
