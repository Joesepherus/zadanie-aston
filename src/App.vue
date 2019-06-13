<template>
  <div id="app">
    <div class="main">
      <h2>Kalkulačka poistenia</h2>
      <p class="error">{{error}}</p>
      <CustomSelect
        placeholder="vyberte variant poistenia"
        label="variant poistenia"
        required="{true}"
        type="insurance"
        v-bind:options="insuranceOptions"
        @change="handleChange"
      ></CustomSelect>
      <CustomDatePicker
        placeholder="vyberte začiatok poistenia"
        label="začiatok postenia"
        required="{true}"
        @change="handleChange"
        type="dateStart"
        v-bind:defaultDate="dateStart"
      ></CustomDatePicker>
      <CustomDatePicker
        placeholder="vyberete koniec poistenia"
        label="koniec poistenia"
        required="{true}"
        @change="handleChange"
        type="dateEnd"
        v-bind:defaultDate="dateEnd"
        v-if="insurance === 'shortterm'"
      ></CustomDatePicker>
      <CustomSelect
        placeholder="vyberte typ balíka"
        label="typ balíka"
        required="{true}"
        v-bind:options="packageOptions"
        type="package"
        @change="handleChange"
      ></CustomSelect>
      <CustomCheckbox
        label="storno"
        type="storno"
        @change="handleChange"
        v-bind:defaultValue="storno"
      ></CustomCheckbox>
      <CustomCheckbox
        label="špotové aktivity"
        type="sport_activity"
        @change="handleChange"
        v-bind:defaultValue="sport_activity"
      ></CustomCheckbox>
      <CustomInput
        placeholder="zadajte počet osôb"
        label="počet osôb"
        required="{true}"
        type="people_count"
        @change="handleChange"
        v-bind:defaultValue="people_count"
      ></CustomInput>
      <div class="btn-calc">
        <CustomButton text="vypočítaj poistenie" @click="calculateInsurance"></CustomButton>
      </div>
      <div class="resultContainer" v-if="result !== ''">
        <span>výsledok</span>
        <span class="result">{{result}} €</span>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import CustomButton from "./components/basic/CustomButton.vue";
import CustomSelect from "./components/basic/CustomSelect";
import CustomDatePicker from "./components/basic/CustomDatePicker";
import CustomCheckbox from "./components/basic/CustomCheckbox";
import CustomInput from "./components/basic/CustomInput";

let today = new Date();
var date = new Date(
  today.getFullYear() + "-" + (today.getMonth() + 1) + "-" + today.getDate()
);
export default {
  name: "app",
  data() {
    return {
      insurance: "",
      dateStart: date,
      dateEnd: date,
      package: "",
      storno: true,
      sport_activity: true,
      people_count: "1",
      error: "",
      result: "",
      insuranceOptions: [
        {
          text: "krátkodobe",
          value: "shortterm"
        },
        {
          text: "celoročne",
          value: "yearly"
        }
      ],
      packageOptions: [
        {
          text: "zakladný",
          value: 0,
          shortterm_price: 1.2,
          yearly_price: 39
        },
        {
          text: "rozšírený",
          value: 1,
          shortterm_price: 1.8,
          yearly_price: 49
        },
        {
          text: "extra",
          value: 2,
          shortterm_price: 2.4,
          yearly_price: 59
        }
      ],
      stornoOptions: {
        shortterm_price: 1.5,
        yearly_price: 1.3
      },
      sport_activityOptions: {
        shortterm_price: 1.3,
        yearly_price: 1.1
      }
    };
  },
  methods: {
    handleChange(value, type) {
      this[type] = value;
      if (type === "people_count") {
        // this[type] = parseInt(value);
      }
    },
    calculateInsurance() {
      this.result = "";
      if (this.insurance === "") {
        this.error = "Musíte vybrať variant poistenia.";
        return;
      }
      if (isNaN(this.people_count) || this.people_count < 1) {
        this.error = "Počet osôb musí byť kladné číslo.";
        return;
      }

      let days;
      if (this.insurance === "shortterm") {
        days = moment(this.dateEnd).diff(moment(this.dateStart), "days");

        if (days < 1) {
          this.error = "Konečný dátum musí byť neskôr ako začiatočný.";
          return;
        }
      } else days = 1;
      if (this.package === "") {
        this.error = "Musíte vybrať typ balíka.";
        return;
      }

      this.error = "";

      let total_price =
        days *
        this.packageOptions[this.package][this.insurance + "_price"] *
        this.people_count;

      if (this.storno)
        total_price *= this.stornoOptions[this.insurance + "_price"];
      if (this.sport_activity)
        total_price *= this.sport_activityOptions[this.insurance + "_price"];

      this.result = total_price.toFixed(2);
    }
  },
  components: {
    CustomButton,
    CustomSelect,
    CustomDatePicker,
    CustomCheckbox,
    CustomInput
  }
};
</script>

<style>
lo #app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 350px;
  max-width: 300px;
  margin: auto;
}

.main {
  padding: 20px 40px 20px 40px;
  border: 1px solid rgba(34, 36, 38, 0.15);
  border-radius: 10px;
  outline: none;
  width: 100%;
  background: white;
}

.main > div {
  margin-top: 20px;
}

.error {
  color: red;
  font-weight: 600;
}

.result {
  padding-left: 20px;
  font-weight: 700;
  color: blue;
  font-size: 20px;
}

.required {
  color: red;
  padding-right: 5px;
}

.ui.selection.dropdown {
  width: 100%;
}

.btn-calc {
  display: flex;
  justify-content: center;
}

.resultContainer {
  text-align: center;
}

p {
  margin-bottom: 5px;
}

body {
  padding: 0px;
  background-image: linear-gradient(#abb6f4, #05d99f);
}
</style>
