<template>
  <div
    class="interactive"
    v-bind:class="{
      safeToShred: shredDate && safeToShred,
      unsafeToShred: shredDate && !safeToShred
    }"
  >
    <p>When was the patient's <strong>date of birth?</strong></p>
    <datepicker
      v-model="birthdate"
      v-bind:typeable="true"
      v-bind:clear-button="true"
      v-bind:use-utc="true"
    ></datepicker>

    <p>When was the <strong>last visit?</strong></p>
    <datepicker
      v-model="lastVisit"
      v-bind:typeable="true"
      v-bind:clear-button="true"
      v-bind:use-utc="true"
    ></datepicker>

    <div v-if="shredDate">
      <p>
        Shred date: <strong>{{ format(shredDate) }}</strong>
      </p>
      <p v-if="safeToShred">Safe to shred!</p>
      <p v-if="!safeToShred">Not safe to shred.</p>
    </div>
  </div>
</template>

<script>
import Datepicker from "vuejs-datepicker";
import addYears from "date-fns/addYears";
import differenceInYears from "date-fns/differenceInYears";
import max from "date-fns/max";
import isSameDay from "date-fns/isSameDay";
import format from "date-fns/format";
import isBefore from "date-fns/isBefore";

export default {
  name: "Interactive",
  components: {
    Datepicker
  },

  data: function() {
    return {
      lastVisit: new Date(),
      birthdate: new Date()
    };
  },

  methods: {
    format: function(date) {
      return format(date, "MMMM do, yyyy");
    }
  },

  computed: {
    shredDate: function() {
      if (
        isBefore(this.lastVisit, this.birthdate) ||
        isSameDay(this.lastVisit, this.birthdate) ||
        isSameDay(this.birthdate, new Date()) ||
        isSameDay(this.lastVisit, new Date()) ||
        !this.lastVisit ||
        !this.birthdate
      ) {
        return null;
      }

      if (differenceInYears(this.lastVisit, this.birthdate) >= 18) {
        return addYears(this.lastVisit, 7);
      } else {
        const sevenYearsFromLastEncounter = addYears(this.lastVisit, 7);
        const eighteenthBirthday = addYears(this.birthdate, 18);
        
        // console.log(
        //   sevenYearsFromLastEncounter,
        //   eighteenthBirthday,
        //   max([sevenYearsFromLastEncounter, eighteenthBirthday])
        // );
        
        return max([sevenYearsFromLastEncounter, eighteenthBirthday]);
      }
    },

    safeToShred: function() {
      return isBefore(this.shredDate, new Date());
    }
  }
};
</script>

<style>
.interactive {
  border: 1px solid gray;
  padding: 1em;
  margin-bottom: 3rem;
}

.interactive > p:first-child {
  margin-top: 0;
}

.safeToShred {
  background: hsla(130, 86%, 69%, 1)
}

.unsafeToShred {
  background: hsla(0, 86%, 69%, 1)
}
</style>
