<template>
    <div id="app">
        <div
            class="date-picker-toggle-btn cursor-pointer"
            @click="() => showDatePicker = !showDatePicker"
            ref="date-picker-toggle-button"
        >
            <div>{{formatDate(startDate)}}</div>
            <div v-if="endDate" class="date-interval-separator">
                <img src="@/components/datePicker/icons/arrow.svg" alt="">
            </div>
            <div>
                <div v-if="endDate">{{formatDate(endDate)}}</div>
            </div>
        </div>
        <div
            class="date-picker-container"
            v-if="showDatePicker"
            v-clickOutside="{
                exclude: ['date-picker-toggle-button'],
                handler: 'closeDatePicker'
            }"
        >
            <DatePicker
                :options="datePickerOptions"
                :dateOne="startDate"
                :dateTwo="endDate"
                :confirmByButton="true"
                :confirmed="confirmDateChosen"
                @startDateSelected="(startDateValue) => startDate = startDateValue"
                @endDateSelected=" (endDateValue) => endDate = endDateValue"
                @closeDatePicker="closeDatePicker()"
            />
            <div class="btns-container">
                <div class="btn cursor-pointer" @click="showDatePicker = !showDatePicker">cancel</div>
                <div class="btn cursor-pointer" @click="confirmDate()">ok</div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import DatePicker from "./components/datePicker/DatePicker.vue";
import {
  formatDateToString,
  addDays
} from "./components/datePicker/helpers/dateFunctions";

@Component({
  components: {
    DatePicker
  }
})
export default class App extends Vue {
  private datePickerOptions = {
    monthNames: [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December"
    ],
    monthNamesShort: [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec"
    ],
    weekDaysShort: ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"]
  };

  private showDatePicker = true;
  private startDate: Date | null = new Date();
  private endDate: Date | null = addDays(new Date(), 4);
  private confirmDateChosen = false;

  private formatDate(date: Date) {
    return formatDateToString(date, this.datePickerOptions.monthNamesShort);
  }

  private confirmDate() {
    this.confirmDateChosen = true;
  }

  closeDatePicker() {
    this.showDatePicker = false;
    this.confirmDateChosen = false;
  }
}
</script>

<style lang="scss">

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .date-picker-toggle-btn {
        min-width: 200px;
        max-width: 240px;
        display: flex;
        justify-content: center;
        align-items: center;
        border: 1px solid #ababab;
        padding: 0.8rem;
        border-radius: 4px;
        margin: 0 auto;
        .date-interval-separator {
            height: 16px;
            width: 20px;
            margin: 0 4px;
            img {
                width: 100%;
                height: 100%;
            }
        }
    }

    .date-picker-container {
        margin: 1rem auto;
        box-shadow: 4px 4px 12px 4px rgba(95, 101, 114, 0.25);
        border-radius: 4px;
        max-width: 600px;
        .btns-container {
            display: flex;
            justify-content: center;
            align-items: center;
            padding-bottom: 1rem;
            .btn {
                padding: 0.4rem 0.8rem;
                color: #195d7a;
                font-weight: bold;
                margin: 0 0.5rem;
            }
        }
    }

    .cursor-pointer {
        cursor: pointer;
    }

</style>
