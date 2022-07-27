
<script>
// logic goes here

import { ref } from 'vue';
import * as toxicity from '@tensorflow-models/toxicity'
import * as tfjs from '@tensorflow/tfjs'
import { any } from '@tensorflow/tfjs-core';
// const textMss = ref('')
const threshold = 0.9
export default 
{

  data()
  {
    return { returnedPredictions : [],
    txtMss: ''
    }
},
  methods : {
   checkTox()
   {
 //   if(textMss.value.length > 0){
    //  tensorflow code
  toxicity.load(threshold,[]).then(model => {
  model.classify(this.txtMss).then(predictions => {
    // `predictions` is an array of objects, one for each prediction head,
    // that contains the raw probabilities for each input along with the
    // final prediction in `match` (either `true` or `false`).
    // If neither prediction exceeds the threshold, `match` is `null`.
    predictions.forEach(prediction => {
      if (prediction) {
     this.returnedPredictions.push(prediction)
      }
    })

    console.log(this.returnedPredictions)
    });
   
 
    
    /*
    prints:
    {
      "label": "identity_attack",
      "results": [{
        "probabilities": [0.9659664034843445, 0.03403361141681671],
        "match": false
      }]
    },
    {
      "label": "insult",
      "results": [{
        "probabilities": [0.08124706149101257, 0.9187529683113098],
        "match": true
      }]
    },
    ...
     */
  });
    //}
    
    // else{
    //    alert("empty text")
    // }


}
  }
}

</script>
<template>
<div>
  <h1>TextTox</h1>
<h4>Check if the text is toxic</h4>
<input v-model="textMss">
<button @click="checkTox">Check</button>
&nbsp;<span>{{textMss}}</span>
<h4>Predictions</h4>
<!-- <div v-if="returnedPredictions"> -->
<li v-for="pred in returnedPredictions">
<span>{{pred.label}} : {{pred.results[0].match}}, probabilities: {{pred.results[0].probabilities}}</span>
</li>
</div>

<!-- </div> -->
</template>