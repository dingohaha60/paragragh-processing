<template>
  <!--  <img alt="Vue logo" src="./assets/logo.png">-->
  <div>
    <table style="width: 80%">
      <td style="width: 70%">
        <textarea v-model="thePara" style="width: 97%"></textarea>
        <br />
        <button @click="transfrom">转换</button>
        <br />
        <textarea v-model="theResult.str" style="width: 97%"></textarea>
      </td>
      <td style="width: 20%; vertical-align: top">
        <textarea v-model="theEnds" style="height: 200px"></textarea>
      </td>
    </table>
  </div>
  <!--  <HelloWorld msg="Welcome to Your Vue.js App"/>-->
</template>

<script setup>
import { reactive, ref } from "vue";

// let aa="";
let thePara = ref("");
let theEnds = ref("");
let theResult = reactive({});

function transfrom() {
  console.log("thePara", thePara);
  let splitted = thePara.value.split("\n");
  let theEndsSplits = theEnds.value.split("\n");
  let jointedThePara = splitted.join(" ");

  // jointedThePara.split();
  let thePattern = "";
  thePattern = new RegExp(
    ("(" + theEndsSplits.join(")|(") + ")").replaceAll(".", "\\."),
    "g"
  );

  // thePattern = thePattern.substring(0,thePattern.length-1);
  let matches = jointedThePara.matchAll(thePattern);
  let preMatch = [];
  preMatch["index"] = 0;
  preMatch.push("");
  let theResultStr = "";
  for (const match of matches) {
    // console.log("the result splits",thePattern.substring(match.index))
    theResultStr =
      theResultStr +
      jointedThePara
        .substring(
          preMatch.index + preMatch[0].length,
          match.index + match[0].length
        )
        .trim() +
      "\n\n";
    // console.log("the result splits:",theResultStr);
    preMatch = match;
  }

  if (preMatch.index + preMatch[0].length != jointedThePara.length) {
    theResultStr += jointedThePara.substring(
      preMatch.index + preMatch[0].length,
      jointedThePara.length
    );
  }

  theResultStr = theResultStr
    .replaceAll("’", "'")
    .replaceAll("“", '"')
    .replaceAll("”", '"');

  theResult.str = theResultStr;
}

// import HelloWorld from './components/HelloWorld.vue''
//
// export default {
//   name: ''App'',
//   components: {
//     HelloWorld
//   }
// }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 20px;
}

textarea {
  width: 80%;
  height: 400px;
  resize: vertical;
  border: #2c3e50 2px solid;
  font-size: 17px;
}

td {
  border: #42b983 1px solid;
}

button {
  width: 120px;
  height: 40px;
}
</style>
