<template>
  <div id="app">
    <h1>Can I Shred It?</h1>
    <p class="subtitle">A Medical Records Game, made by James Little for Susan Laster</p>
    <p>
      Records for adult patients must be maintained for a minimum of seven years
      from date of last patient encounter.
    </p>
    <p>
      If a patient is a minor on the date of the last visit, then the physician
      must maintain the pediatric patient’s records for a minimum period of
      either
    </p>

    <ul>
      <li>seven years from the date of the last patient encounter or</li>
      <li>until the patient reaches the age of eighteen,</li>
    </ul>

    <p>whichever is the longer retention period.</p>

    <Interactive />
    
    <p class="subtitle">Fine print: This isn't legally binding. It doesn't store any data except the fact that you viewed the page.<br>&copy; James Little 2020</p>
  </div>
</template>

<script>
import Interactive from "./components/Interactive.vue";

export default {
  name: "app",
  components: {
    Interactive
  }
};
</script>

<style>
body {
  width: 95%;
  max-width: 700px;
  margin: 0 auto;
  padding: 1em 0;
  font-family: sans-serif;
  line-height: 1.4
}

h1 {
  margin-bottom: 0;
}

.subtitle {
  margin-top: 0;
  font-size: 0.8em;
  font-style: italic;
}
</style>