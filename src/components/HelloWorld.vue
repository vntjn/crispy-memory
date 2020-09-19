<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <b><pre style="font-size: 16px; font">
      कोर्स चुनें:      <input type="radio" name="courseType" @click="showInputs('grading')">ग्रेडिंग      <input type="radio" name="courseType" @click="showInputs('nonGrading')">नॉन-ग्रेडिंग      <input type="radio" name="courseType" @click="showInputs('diplomaPharmacy')">डिप्लोमा फार्मेसी
      </pre></b>
      <div v-show="showInputFields">
        <h3>ऑनलाइन एक्जाम में प्राप्त मार्क्स</h3>
        <input type="number" step=".01" v-model="onlineExamMarks" @input="showResult = false"/> <br /> <br />
        <h3> {{ resultEntryType }} </h3>
        <input type="number" step=".01" v-model="semSGPA" @input="showResult = false"/> <br /> <br />
        <button @click="calculateResult">रिज़ल्ट</button>
      </div>
      <div v-if="showResult">
      <h3>फ़ाइनल थियोरी मार्क्स</h3> {{ finalTheoryMarks }} <sup style="color:red">*</sup> <br /> <br />
      <h3>थियोरी रिज़ल्ट</h3>
      <span :style="resultColor">
        <b>{{ theoryResult }}</b>
      </span> <br> <br>
      <span style="color:red"><sup>*</sup>{{alert}}</span>
      </div>
    </div>
    <div class="footer">&#169; Designed and created by VinJai</div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      msg:
        "RGPV पोलिटेकनिक के ऑनलाइन परीक्षा का रिज़ल्ट जानने के लिए पूछी गई जानकारी को उसके नीचे वाले बक्से मे भरने पर रिज़ल्ट बताएगा।",
      alert: "यह सिर्फ थियोरी पेपर का रिज़ल्ट दर्शाता है",
      onlineExamMarks: null,
      semSGPA: null,
      resultEntryType: '',
      finalTheoryMarks: null,
      theoryResult: null,
      showResult: false,
      showInputFields: false,
      courseType: ''
    };
  },
  methods: {
    calculateResult() {
      this.showResult = true;
      if(this.courseType === 'grading') {
        if(this.onlineExamMarks && this.semSGPA){
        this.finalTheoryMarks = this.onlineExamMarks >=0 && this.onlineExamMarks <=70 && this.semSGPA >=0 && this.semSGPA <=10
          ? (this.onlineExamMarks / 2 + (7 * this.semSGPA) / 2).toFixed(2)
          : "एंट्री अमान्य";
        } else {this.finalTheoryMarks = "दोनों बॉक्स भरें"}
        this.theoryResult= this.finalTheoryMarks === "एंट्री अमान्य"
          ? "एंट्री अमान्य"
          : this.finalTheoryMarks == "दोनों बॉक्स भरें" ? "दोनों बॉक्स भरें"
          : this.finalTheoryMarks >= 22
          ? "PASS"
          : "FAIL";
      }
      else if(this.courseType === 'nonGrading') {
        if(this.onlineExamMarks && this.semSGPA){
        this.finalTheoryMarks = this.onlineExamMarks >=0 && this.onlineExamMarks <=70 && this.semSGPA >=0 && this.semSGPA <=10
          ? ((5 * this.onlineExamMarks) / 7 + (this.semSGPA) / 2).toFixed(2)
          : "एंट्री अमान्य";
        } else {this.finalTheoryMarks = "दोनों बॉक्स भरें"}
        this.theoryResult= this.finalTheoryMarks === "एंट्री अमान्य"
          ? "एंट्री अमान्य"
          : this.finalTheoryMarks == "दोनों बॉक्स भरें" ? "दोनों बॉक्स भरें"
          : this.finalTheoryMarks >= 33
          ? "PASS"
          : "FAIL";
      }
      else if(this.courseType === 'diplomaPharmacy') {
        if(this.onlineExamMarks && this.semSGPA){
        this.finalTheoryMarks = this.onlineExamMarks >=0 && this.onlineExamMarks <=70 && this.semSGPA >=0 && this.semSGPA <=100
          ? (this.onlineExamMarks / 2 + (2 * this.semSGPA) / 5).toFixed(2)
          : "एंट्री अमान्य";
        } else {this.finalTheoryMarks = "दोनों बॉक्स भरें"}
        this.theoryResult= this.finalTheoryMarks === "एंट्री अमान्य"
          ? "एंट्री अमान्य"
          : this.finalTheoryMarks == "दोनों बॉक्स भरें" ? "दोनों बॉक्स भरें"
          : this.finalTheoryMarks >= 27
          ? "PASS"
          : "FAIL";
      }
    },
    showInputs(course){
      this.onlineExamMarks = null;
      this.semSGPA = null;
      this.resultEntryType = '';
      this.finalTheoryMarks = null;
      this.theoryResult = null;
      this.showResult = false;
      if(course === 'grading') {
        this.resultEntryType = 'पिछले sem का SGPA';
        this.courseType = course;
      }
      else if(course === 'nonGrading') {
        this.resultEntryType = 'पिछले sem का SGPA';
        this.courseType = course;
      }
      else if(course === 'diplomaPharmacy') {
        this.resultEntryType = '% रिज़ल्ट (प्रथम वर्ष का)';
        this.courseType = course;
      }
        this.showInputFields = true;
    }
  },
  computed : {
    resultColor() {
      return this.theoryResult === "PASS" ? "color: green" : "color: red";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.footer {
  height: 15px;
  position: fixed;
  bottom: 0;
  background-color: white;
  font-size: 12px;
}
</style>
