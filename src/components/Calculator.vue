<script lang="ts" setup>
import { ref } from "vue";

const byteUnits: string[] = [
  "bytes (B)",
  "kilobytes (kB)",
  "megabytes (MB)",
  "gigabytes (GB)",
  "terabytes (TB)",
  "petabytes (PB)",
];
const bitUnits: string[] = [
  "bits (b)",
  "kilobits (kb)",
  "megabits (Mb)",
  "gigabits (Gb)",
  "terabits (Tb)",
];
const allUnits: string[] = byteUnits.concat(bitUnits);

const fileSize = ref();
const fileUnit = ref(allUnits[3]);

const downloadSpeed = ref();
const speedUnit = ref(allUnits[8]);

let downloadTime = ref("Input values and then calculate.");

function convertToBit(num: number, unit: string): number {
  const unitType: string =
    allUnits.indexOf(unit) <= byteUnits.length ? "byte" : "bit";
  const unitIndex: number =
    unitType === "byte" ? byteUnits.indexOf(unit) : bitUnits.indexOf(unit);

  const multiplier: number = Math.pow(1000, unitIndex);
  const value: number = num * multiplier;

  const answer = unitType === "byte" ? value * 8 : value;

  return answer;
}

function FormatTime(secondsInput: number): string {
  const days: number = Math.floor(secondsInput / 84600);
  const hours: number = Math.floor((secondsInput % 86400) / 3600);
  const minutes: number = Math.floor(((secondsInput % 86400) % 3600) / 60);
  const seconds: number = Math.floor(((secondsInput % 86400) % 3600) % 60);

  const daysString: string =
    days > 0 ? days + (days === 1 ? " day " : " days ") : "";
  const hoursString: string =
    hours > 0 ? hours + (hours === 1 ? " hour " : " hours ") : "";
  const minutesString: string =
    minutes > 0 ? minutes + (minutes === 1 ? " minute " : " minutes ") : "";
  const secondsString: string =
    seconds > 0 ? seconds + (seconds === 1 ? " second " : " seconds ") : "";

  return daysString + hoursString + minutesString + secondsString;
}

function calculateDownloadTime() {
  const fileInBits: number = convertToBit(fileSize.value, fileUnit.value);
  const speedInBits: number = convertToBit(
    downloadSpeed.value,
    speedUnit.value
  );
  const downloadTimeInSeconds = fileInBits / speedInBits;

  downloadTime.value = FormatTime(downloadTimeInSeconds);
}
</script>

<script lang="ts">
export default {
  name: "Calculator",
};
</script>

<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="8" md="6" lg="4">
        <h1>Download Time Calculator</h1>
        <h4>Made by <a href="https://github.com/NotSuako">Suako</a></h4>
      </v-col>
    </v-row>

    <v-row no-gutters justify="center" class="ma-3">
      <v-col sm="4" md="3" lg="2">
        <v-text-field label="File size" v-model="fileSize" />
      </v-col>
      <v-col sm="4" md="3" lg="2">
        <v-select label="Select unit" :items="allUnits" v-model="fileUnit" />
      </v-col>
    </v-row>

    <v-row no-gutters justify="center" class="ma-3">
      <v-col sm="4" md="3" lg="2">
        <v-text-field label="Download speed" v-model="downloadSpeed" />
      </v-col>
      <v-col sm="4" md="3" lg="2">
        <v-select label="Select unit" :items="allUnits" v-model="speedUnit" />
      </v-col>
    </v-row>

    <v-row justify="center" class="ma-3">
      <v-col sm="8" md="6" lg="4">
        <v-btn block @click="calculateDownloadTime">Calculate</v-btn>
      </v-col>
    </v-row>

    <v-row justify="center" class="ma-3">
      <v-col class="text-center">
        <h4>{{ downloadTime }}</h4>
      </v-col>
    </v-row>
  </v-container>
</template>

<style></style>
