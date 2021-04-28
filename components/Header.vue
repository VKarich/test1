<template>
  <div class="selection">
    <section>
      <b-steps
        size="is-small"
        v-model="activeStep"
        :animated="isAnimated"
        :rounded="isRounded"
        :has-navigation="hasNavigation"
        :icon-prev="prevIcon"
        :icon-next="nextIcon"
        :label-position="labelPosition"
        :mobile-mode="mobileMode"
      >
        <b-step-item
          step="1"
          label="Выбор договора"
          :clickable="isStepsClickable"
        >
        <p>Твой личный бизнес-партнёр</p>
          <h1 class="title has-text-centered">Выберите договор</h1>

          <b-field class="select_field">
            <b-select :placeholder="currentMenuAgr.msg" @input="selection($event)">
              <optgroup label="Недвижимость">
                <option
                  v-for="(agrmt, index) in agrmns"
                  :key="index"
                  :value="agrmt"
                >
                  {{ agrmt.msg }}
                </option>
              </optgroup>
            </b-select>
          </b-field>
        </b-step-item>

        <b-step-item
          step="2"
          label="Участники договора"
          :clickable="isStepsClickable"
          :type="{ 'is-success': isProfileSuccess }"
        >
          <h1 class="title has-text-centered">Выберите участников договора</h1>
          <b-field class="select_field" label="В качестве кого Вы хотели бы заключить договор?">

            <b-tooltip
            label="Физическое лицо - Вы действуете от своего имени. Юридическое лицо - Вы действуете от имени Организации"
            position="is-bottom"
            size="is-large"
            multilined>
            <b-select :placeholder="currentMenuCo1.msg" @input="selection_2($event)">
                <option
                 v-for="(donor, index) in donors"
                :key="index"
                :value="donor"
                >
                  {{ donor.msg }}
                </option>
            </b-select>
            </b-tooltip>
          </b-field>
          <b-field class="select_field" label="С кем Вы планируете заключить договор?">

            <b-tooltip
            label="Физическое лицо - вторая сторона действует от своего имени. Юридическое лицо - вторая сторона действует от имени Организации"
            position="is-bottom"
            size="is-large"
            multilined>

            <b-select :placeholder="currentMenuCo2.msg" @input="selection_3($event)">
                <option
                 v-for="(recipient, index) in recipients"
                :key="index"
                :value="recipient"
                >
                  {{ recipient.msg }}
                </option>
            </b-select>
            </b-tooltip>
          </b-field>
        </b-step-item>

        <b-step-item
          step="3"
          :visible="showAgrmt"
          label="Заполнение договора"
          :clickable="isStepsClickableAgrmt"
        >
          <div class="generform">
              <generform
              :show_first="show_first"
              :select_first_step="select_agreement"
              :select_second_step="select_from"
              :select_third_step="select_to"
            />
          </div>
        </b-step-item>
      </b-steps>
    </section>
  </div>
</template>

<script>
/* eslint-disable */
import Generform from "./Generform";

export default {
  components: { Generform },
  name: "Header",
  data() {
    return {
      activeStep: 0,
      activeSecond: 0,
      show_navi: false,
      showAgrmt: true,
      isAnimated: false,
      isRounded: true,
      isStepsClickable: false,
      isStepsClickableAgrmt: false,

      hasNavigation: false,
      customNavigation: false,
      isProfileSuccess: false,

      prevIcon: "chevron-left",
      nextIcon: "chevron-right",
      labelPosition: "bottom",
      mobileMode: "compact",

      onSelect: true,
      activeTab: 0,
      isScrollable: false,
      maxHeight: 200,
      currentMenuAgr: { id: 1, msg: "Список доступных договоров" },
      currentMenuCo1: { id: 1, msg: "Выберите вариант" },
      currentMenuCo2: { id: 1, msg: "Выберите вариант" },
      isPublic: true,
      show_first: true,
      select_agreement: "",
      select_from: "",
      select_to: "",
      agrmns: [
        {
          id: 1,
          msg: "Договор купли-продажи недвижимого имущества",
        },
      ],
      donors: [
        {
          id: 1,
          msg: "Физическое лицо",
          value: "ФЛ",
        },
        {
          id: 2,
          msg: "Юридическое лицо",
          value: "ЮЛ",
        },
      ],
      recipients: [
        {
          id: 1,
          msg: "Физическое лицо",
          value: "ФЛ",
        },
        {
          id: 2,
          msg: "Юридическое лицо",
          value: "ЮЛ",
        },
      ],
    };
  },
//   created() {
//     if (
//       this.$route.query.agrmt &&
//       JSON.stringify(this.agrmns).match(this.$route.query.agrmt)
//     ) {
//       this.select_agreement = this.$route.query.agrmt;
//       this.currentMenuAgr.msg = this.$route.query.agrmt;
//     }
//   },
  methods: {
    selection(event) {
      this.select_agreement = event.msg;
    //   this.$router
    //     .push({ query: { agrmt: this.select_agreement } })
    //     .catch(() => {});
        this.activeStep +=1
        this.hasNavigation = true
        this.isStepsClickable = true
        this.hasNavigation = false
    },
    selection_2(event) {
      this.select_from = event.value;
      this.activeSecond += 1;
    },
    selection_3(event) {
      this.select_to = event.value;
      this.show_first = false;
      this.onSelect = false;
      this.activeSecond += 1;
      if (this.activeSecond == 2) {
        this.activeStep = 2
      }
      this.isStepsClickableAgrmt = true
      this.hasNavigation = true
    },
  }
};
</script>

<style>
.main {
  text-align: center;
}
.selection {
  padding-top: 20px;
  text-align: center;
}
.select_field {
  text-align: center;
}
p {
  padding: 4px;
}
.media-left {
  margin-right: 0rem;
  margin-left: 0rem;
}
.dropdown-item,
.dropdown .dropdown-menu .has-link a {
  color: #4a4a4a;
  display: block;
  font-size: 0.875rem;
  line-height: 1.5;
  width: auto;
  padding: 0.375rem 3rem;
  position: relative;
}
.section_btn {
  padding-bottom: 1%;
}
</style>


