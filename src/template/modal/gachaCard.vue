<template>
<div v-show="show" class="modal">
  <transition name="scale" @after-leave="afterLeave">
    <div style="width:900px" v-show="visible">
      <div class="modal-header">
        <title-dot v-once></title-dot>
        <h4 class="modal-title">{{information.name ? information.name : ""}}</h4>
        <small-tab class="pull-right" :tab="practice" v-model="currentPractice" @tabClicked="toggle"></small-tab>
      </div>
      <div class="modal-body" :style="{ maxHeight: bodyMaxHeight }">
        <table class="table-bordered" border="1" :class="{
          cute: information.charaData ? information.charaData.type === 1 : false,
          cool: information.charaData ? information.charaData.type === 2 : false,
          passion: information.charaData ? information.charaData.type === 3 : false
        }">
          <tr>
            <td width="15%">{{$t("idol.id")}}</td>
            <td width="45%">{{information.id}}</td>
            <td width="15%">{{$t("idol.okurigana")}}</td>
            <td width="25%">{{information.charaData ? information.charaData.name_kana : ""}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.card_name")}}</td>
            <td>{{information.name}}</td>
            <td>{{$t("idol.name")}}</td>
            <td>{{information.charaData ? information.charaData.name : ""}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.chara_id")}}</td>
            <td>{{information.chara_id}}</td>
            <td>{{$t("idol.age")}}</td>
            <td>{{information.charaData ? information.charaData.age : ""}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.rarity")}}</td>
            <td>{{rarity}}</td>
            <td>{{$t("idol.height")}}</td>
            <td>{{information.charaData ? information.charaData.height : ""}}</td>
          </tr>
          <tr>
            <td class="hp">{{$t("idol.hp")}}</td>
            <td class="hp">{{hp}}</td>
            <td>{{$t("idol.weight")}}</td>
            <td>{{information.charaData ? information.charaData.weight : ""}}</td>
          </tr>
          <tr>
            <td class="vocal">{{$t("idol.vocal")}}</td>
            <td class="vocal">{{vocal}}</td>
            <td>{{$t("idol.birth")}}</td>
            <td>{{information.charaData ? (information.charaData.birth_month + "月" + information.charaData.birth_day + "日") : ""}}</td>
          </tr>
          <tr>
            <td class="dance">{{$t("idol.dance")}}</td>
            <td class="dance">{{dance}}</td>
            <td>{{$t("idol.blood")}}</td>
            <td>{{information.charaData ? information.charaData.blood_type : "" | blood}}</td>
          </tr>
          <tr>
            <td class="visual">{{$t("idol.visual")}}</td>
            <td class="visual">{{visual}}</td>
            <td>{{$t("idol.handedness")}}</td>
            <td>{{information.charaData ? information.charaData.hand : "" | hand}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.solo_live")}}</td>
            <td>{{solo}}</td>
            <td>{{$t("idol.threesize")}}</td>
            <td>{{information.charaData ? [information.charaData.body_size_1, information.charaData.body_size_2, information.charaData.body_size_3] : [] | threesize}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.skill_name")}}</td>
            <td>{{information.skill ? information.skill.skill_name : ""}}</td>
            <td>{{$t("idol.hometown")}}</td>
            <td>{{information.charaData ? information.charaData.hometown : ""}}</td>
          </tr>
          <tr>
            <!-- <td>{{$t("idol.skill_explain")}}</td> -->
            <td colspan="2" style="text-align:left">{{information.skill ? information.skill.explain : ""}}</td>
            <td>{{$t("idol.constellation")}}</td>
            <td>{{information.charaData ? information.charaData.seiza : ""}}</td>
          </tr>
          <tr>
            <td>{{$t("idol.leader_skill_name")}}</td>
            <td>{{information.leaderSkill ? information.leaderSkill.name : ""}}</td>
            <td>{{$t("idol.voice")}}</td>
            <td>{{information.charaData ? information.charaData.voice : ""}}</td>
          </tr>
          <tr>
            <!-- <td>{{$t("idol.leader_skill_explain")}}</td> -->
            <td colspan="2" style="text-align:left">{{information.leaderSkill ? information.leaderSkill.explain : ""}}</td>
            <td>{{$t("idol.favorite")}}</td>
            <td>{{information.charaData ? information.charaData.favorite : ""}}</td>
          </tr>
          <tr v-if="card.limited">
            <td>{{$t("idol.limited")}}</td>
            <td colspan="3">
              <p v-for="limit in card.limited">{{"(" + (limit.id > 9999 ? $t("idol.gacha") + limit.id : $t("idol.event") + limit.id) + ") " + limit.name +
                ": " + limit.startDate + " ～ " + limit.endDate}}</p>
            </td>
          </tr>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="cgss-btn cgss-btn-default" @click="close">{{$t("home.close")}}</button>
      </div>
    </div>
  </transition>
</div>
</template>

<script>
import smallTab from '../component/smallTab.vue'
import vmIdol from '../../js/renderer/idol.js'
import modalMixin from '../../js/renderer/modalMixin.js'
export default {
  mixins: [modalMixin],
  components: {
    smallTab
  },
  data () {
    return {
      card: {},
      cardPlus: {},
      information: {},
      currentPractice: 'idol.before',
      practice: {
        be: 'idol.before',
        af: 'idol.after'
      }
    }
  },
  props: {
    'master': {
      type: Object,
      require: true
    }
  },
  filters: vmIdol.filters,
  computed: vmIdol.computed,
  methods: {
    toggle (practice) {
      switch (practice) {
        case 'idol.before':
          this.information = this.card
          break
        case 'idol.after':
          this.information = this.cardPlus
          break
        default:
          break
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.event.$on('showCard', (card) => {
        const cardPlus = this.cardData.filter(c => c.id == card.evolution_id)[0]
        this.currentPractice = 'idol.before'
        this.information = card
        this.card = card
        this.cardPlus = cardPlus
        this.show = true
        this.visible = true
      })
    })
  }
}
</script>
