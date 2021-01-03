<template>
  <svg class="oursvg" :width="width" :height="height">
    <circle
      v-for="d in data"
      :key="d"
      :cx="d.x"
      :cy="d.y"
      r="5"
      fill="#c5a7d9"
    ></circle>
    <g v-if="catVar === 'educ' || catVar === 'educDegree'">
      <g
        v-for="d in calcData()"
        :key="d"
        :transform="`translate(${width / 300}, ${width / 30})`"
      >
        <text
          :x="xScaleEduc(d.category)"
          :y="yScaleEduc(d.category) + popRad(d.num)"
          fill="black"
          text-anchor="middle"
          font-weght="800"
        >
          {{ catVar === "" ? "" : d.category }}
        </text>
      </g>
    </g>
    <g v-else-if="catVar === 'salary'">
      <g
        v-for="d in calcData()"
        :key="d"
        :transform="`translate(${width / 300}, ${width / 30})`"
      >
        <text
          :x="xScaleSalary(d.category)"
          :y="yScaleSalary(d.category) + popRad(d.num)"
          fill="black"
          text-anchor="middle"
          font-weght="800"
        >
          {{ catVar === "" ? "" : d.category }}
        </text>
    </g>
    </g>
    <g v-else>
      <g
        v-for="d in calcData()"
        :key="d"
        :transform="`translate(${width / 300}, ${width / 30})`"
      >
        <text
          :x="xScale(d.category)"
          :y="yScale(d.category) + popRad(d.num)"
          fill="black"
          text-anchor="middle"
          font-weght="800"
        >
          {{ catVar === "" ? "" : d.category }}
        </text>
    </g>
    </g>
  </svg>
</template>

<script>
import * as d3 from "d3";
import dataset from "../../public/datasetDVSSurvey100.json";
import _ from "lodash";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

let width = 500;
let height = 600;

export default {
  data() {
    return {
      width,
      height,
      data: [],
      additionalData: [],
      catVar: ""
    };
  },
  mounted() {
    this.data = dataset;
    this.additionalData = this.calcData();
    this.makeSimulation();

    ScrollTrigger.create({
      trigger: "#chart",
      start: "center center",
      end: () => {
        const height = window.innerHeight;

        return `bottom ${height / 4}px`;
},
      endTrigger: "#step14",
      pin: true,
      pinSpacing: false,
      pinType: "fixed",
      anticipatePin: 0.3
    });

    ScrollTrigger.create({
      trigger: "#step1",
      start: "top 80%",
    });

    ScrollTrigger.create({
      trigger: "#step2",
      start: "top 80%",
      onEnter: this.changeForce,
      onLeaveBack: this.makeSimulationBack,
    });

    ScrollTrigger.create({
      trigger: "#step3",
      start: "top 80%",
      onEnter: this.changeForceColor,
      onLeaveBack: this.changeForce,
    });
    ScrollTrigger.create({
      trigger: "#step4",
      start: "top 80%",
      onEnter: this.changeForceYears,
      onLeaveBack: this.changeForceColorCountry,
    });
    ScrollTrigger.create({
      trigger: "#step5",
      start: "top 80%",
      onEnter: this.changeForceEduc,
      onLeaveBack: this.changeForceYears,
    });
    ScrollTrigger.create({
      trigger: "#step6",
      start: "top 80%",
      onEnter: this.changeForceEducDegree,
      onLeaveBack: this.changeForceEduc,
    });
    ScrollTrigger.create({
      trigger: "#step7",
      start: "top 80%",
      onEnter: this.changeForceEducDegreePrivate,
      onLeaveBack: this.changeForceEducDegreePrivateBack,
    });
      ScrollTrigger.create({
      trigger: "#step8",
      start: "top 80%",
      onEnter: this.changeForceEducDegreePublic,
      onLeaveBack: this.changeForceEducDegreePublicBack,
});

    ScrollTrigger.create({
      trigger: "#step9",
      start: "top 80%",
      onEnter: this.changeForceEducDegreeNonProfit,
      onLeaveBack: this.changeForceEducDegreeNonProfitBack,
    });
    ScrollTrigger.create({
      trigger: "#step10",
      start: "top 80%",
      onEnter: this.changeForceEducGender,
      onLeaveBack: this.changeForceGenderBack,
      });
    ScrollTrigger.create({
      trigger: "#step11",
      start: "top 80%",
      onEnter: this.changeForceLearn,
      onLeaveBack: this.changeForceLearnBack,
    });
    ScrollTrigger.create({
      trigger: "#step12",
      start: "top 80%",
      onEnter: this.changeForceSalary,
      onLeaveBack: this.changeForceSalaryBack,
});
    ScrollTrigger.create({
      trigger: "#step13",
      start: "top 80%",
      onEnter: this.changeForcegenderPay,
      onLeaveBack: this.changeForcegenderPayBack,
    });

    ScrollTrigger.create({
      trigger: "#step14",
      start: "top 80%",
      onEnter: this.changePayDVSTime,
      onLeaveBack: this.changePayDVSTimeBack,
    });
  },

  methods: {

    makeSimulation: function() {
      this.simulation = d3.forceSimulation(this.data) .force('charge',d3.forceManyBody().strength(0.5))
      .force('center', d3.forceCenter(width / 2, height / 2))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .force("x",d3.forceX().x(width/2, height / 2).strength(0.5))
      .force("y",d3.forceY().y(height/2, width / 2).strength(0.5)) .alphaDecay(0.10)
    },
    makeSimulationBack: function() {
    this.catVar = "";

    d3.selectAll("circle")
    .data(this.data)
    .attr("fill", "#c5a7d9")

    d3.forceSimulation(this.data)
    .force('charge',d3.forceManyBody().strength(0.5))
    .force('center', d3.forceCenter(width / 2, height / 2))
    .force("collision",d3.forceCollide(6).strength(1).iterations(5))
    .force("x",d3.forceX().x(width/2, height / 2).strength(0.5))
    .force("y",d3.forceY().y(height/2, width / 2).strength(0.5))
    .alphaDecay(0.10);
    },
    calcData() {
      let countCategory = _.countBy(this.data, d => d[this.catVar]);
      let countCategoryArray = Object.keys(countCategory).map(d => {
        return { category: d, num: countCategory[d] };
      });
      return countCategoryArray;
    },
    calcData1() {
      let countCategory = _.countBy(this.data, d => d.learnDV);
      let countCategoryArray = Object.keys(countCategory).map(d => {
        return { category: d, num: countCategory[d] };
      });
      return countCategoryArray;
    },

    xScale(d) {
      const x = d3.scaleOrdinal()
        .domain(_.uniq(_.map(this.data, this.catVar)))
        .range([100,250,400, 250]);

      return x(d);
    },
    xScale2(d) {
      const x = d3
        .scaleOrdinal()
        .domain(_.uniq(_.map(this.data, this.catVar)))
        .range([250,300,350, 300]);

      return x(d);
    },
    xScale1(d) {
      const x = d3.scaleOrdinal()
        .domain(_.uniq(_.map(this.data, "learnDV")))
        .range([200,300,400, 300]);
      return x(d);
    },
    xScaleEduc(d) {
      const x = d3.scaleOrdinal()
        .domain(_.uniq(_.map(this.data, this.catVar)))
        .range([100, 150, 400, 350, 250]);
      return x(d);
},
    yScale(d) {
      const y = d3.scaleOrdinal()
        .domain(_.uniq(_.map(this.data, this.catVar)))
        .range([250, 400, 250, 100]);
      return y(d);
    },
    yScaleEduc(d) {
      const y = d3
        .scaleOrdinal()
        .domain(_.uniq(_.map(this.data, this.catVar)))
        .range([250, 400, 250, 400, 100]);
      return y(d);
    },
    yScaleSalary(d) {
      const y = d3
        .scaleOrdinal()
        .domain(["60,000- 79,000", "80,000 - 99,000" ,"40,000 - 59,000" , "> 140,000" , "100,000 - 119,000" ,"20,000 - 39,000", "<20,000", "120,000 - 139,000", "Unknown"])
        .range([300, 300,150 , 450 , 300 ,150,150, 450,450]);
      return y(d);
    },
    xScaleSalary(d) {
      const x = d3 .scaleOrdinal()
      .domain(["60,000- 79,000", "80,000 -99,000" ,"40,000 - 59,000" , "> 140,000" , "100,000 - 119,000" ,"20,000 -39,000", "<20,000", "120,000 - 139,000", "Unknown"])
      .range([100, 250, 400, 250, 400, 250, 100, 100, 400, 250]);
      return x(d);
},
    yScale1(d) {
      const y = d3.scaleOrdinal()
        .domain(_.uniq(_.map(this.data, "learnDV")))
        .range([500, 700, 500, 300]);
      return y(d);
    },
    popRad(d) {
      const popRadScale = d3.scaleLinear()
        .domain(d3.extent(this.additionalData, d => d.num))
        .range([30, 65]);
      return popRadScale(d);
    },
    tick: function() {
      this.$forceUpdate();
    },
    colorScaleCountries(d) {
      const colorsCountries = d3.scaleOrdinal()
      .domain(_.uniq(_.map(this.data, "countryRec")))
      .range(["#c5a7d9", "#c5a7d9", "#c5a7d9", "#db6571", "#6dc9a0", "lightgrey", "#db6571", "#FFD524", "#7ea8bf"]);
        return colorsCountries(d);
    },
    changeForce: function() {
      this.catVar = "country"

      d3 .forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d =>this.xScale(d.country)).strength(0.2))
      .force("y",d3.forceY().y(d => this.yScale(d.country)).strength(0.2))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10);

      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.country === "Other" ? "lightgrey" : "#c5a7d9");

      this.simulation.stop();

      // d3.selectAll("circle").attr("class", "transition");

      // this.var = "learnDV";
      this.calcData();
      this.yScale();
      this.xScale();


   //   this.additionalData = _.countBy(this.data, d => d.learnDV);
    },
    changeForceColor: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.country === "Other" ? "lightgrey" : "#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => this.colorScaleCountries(d.countryRec))
    },
    changeForceYears: function() {
      this.catVar = "yearsDV";

      d3.selectAll("circle")
      .data(this.data)
      .attr("fill","#c5a7d9");

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScale(d.yearsDV)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScale(d.yearsDV)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5)).alphaDecay(0.10)
    },
    changeForceColorCountry: function() {
      this.catVar = "country";
      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScale(d.country)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScale(d.country)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)

      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", "#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => this.colorScaleCountries(d.countryRec))
    },
    changeForceEduc: function() {
      this.catVar = "educ";

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScaleEduc(d.educ)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScaleEduc(d.educ)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)
    },
    changeForceEducDegree: function() {
      this.catVar = "educDegree";

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScaleEduc(d.educDegree)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScaleEduc(d.educDegree)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10) .on("tick", this.tick);
    },
    changeForceEducDegreePrivate: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", "#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => d.orgPrivate ==="No" ? "#c5a7d9" : "#FFD524")
    },
    changeForceEducDegreePrivateBack: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.orgPrivate==="No" ? "#c5a7d9" :"#FFD524")
      .transition().duration(500)
      .attr("fill", "#c5a7d9")
    },
    changeForceEducDegreePublic: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.orgPrivate ==="No" ? "#c5a7d9" : "#FFD524")
      .transition().duration(500)
      .attr("fill", d => d.orgPublic ==="No" ? "#c5a7d9" : "#db6571")
    },
    changeForceEducDegreePublicBack: function() {
      d3.selectAll("circle")
        .data(this.data)
        .attr("fill", d => (d.orgPublic === "No" ? "#c5a7d9" : "#db6571"))
        .transition()
        .duration(1000)
        .attr("fill", d => (d.orgPrivate === "No" ? "#c5a7d9" : "#FFD524"));
    },
    changeForceEducGender: function() {

    d3.selectAll("circle")
    .data(this.data)
   .attr("fill", d => d.orgNonProfit ==="No" ? "#c5a7d9" :"#6dc9a0")
    .transition().duration(500)
    .attr("fill", d => d.gender ==="man" ? "#6dc9a0": d.gender==="woman" ? "#db6571" :"#FFD524")
    },
    changeForceGenderBack: function() {
        d3.selectAll("circle")
        .data(this.data)
        .attr("fill", d => d.gender ==="man" ? "#6dc9a0" : d.gender==="woman" ? "#db6571" :"#FFD524")
        .transition().duration(500)
        .attr("fill", d => d.orgNonProfit ==="No" ? "#c5a7d9" :"#6dc9a0")
    },
    changeForceLearn: function() {
      this.catVar = "learn";

      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.gender ==="man" ? "#6dc9a0" :d.gender==="woman" ? "#db6571" :"#FFD524")
      .transition().duration(500)
      .attr("fill", "#c5a7d9")

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScale(d.learn)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScale(d.learn)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)
    },
    changeForceLearnBack: function() {
      this.catVar = "educDegree";

      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", "#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => d.gender ==="man" ? "#6dc9a0" :d.gender==="woman" ? "#db6571" :"#FFD524")

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScaleEduc(d.educDegree)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScaleEduc(d.educDegree)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)
    },
    changeForceSalary: function() {
      this.catVar = "salary";
      this.calcData;

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScaleSalary(d.salary)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScaleSalary(d.salary)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)
    },
    changeForceSalaryBack: function() {
      this.catVar = "learn";
      this.calcData;

      d3.forceSimulation(this.data)
      .force('charge',d3.forceManyBody().strength(2))
      .force("x",d3.forceX().x(d=>this.xScale(d.learn)).strength(0.4))
      .force("y",d3.forceY().y(d =>this.yScale(d.learn)).strength(0.4))
      .force("collision",d3.forceCollide(6).strength(1).iterations(5))
      .alphaDecay(0.10)
    },
    changeForcegenderPay: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", "#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => d.genderPay ==="man" ? "#6dc9a0":d.genderPay ==="woman" ? "#db6571" : "#c5a7d9")
    },
     changeForcegenderPayBack: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.genderPay ==="man" ? "#6dc9a0" :d.genderPay ==="woman" ? "#db6571" :"#c5a7d9")
      .transition().duration(500)
      .attr("fill", "#c5a7d9")
     },
    changePayDVSTime: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.genderPay ==="man" ? "#6dc9a0":d.genderPay ==="woman" ? "#db6571" :"#c5a7d9")
      .transition().duration(500)
      .attr("fill", d => d.roledvsPay==="primary" ? "#9660BA" : d.roledvsPay ==="secondary" ? "#c5a7d9" : "lightgrey")
    },
    changePayDVSTimeBack: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.roledvsPay==="primary" ? "#9660BA" : d.roledvsPay ==="secondary" ? "#c5a7d9" : "lightgrey")
      .transition().duration(500)
      .attr("fill", d => d.genderPay ==="man" ?"#6dc9a0":d.genderPay ==="woman" ? "#db6571" : "#c5a7d9")
  },
    changeForceEducDegreeNonProfit: function() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.orgPublic ==="No" ? "#c5a7d9" : "#db6571")
      .transition().duration(500)
      .attr("fill", d => d.orgNonProfit ==="No" ? "#c5a7d9" :"#6dc9a0")
    },
    changeForceEducDegreeNonProfitBack() {
      d3.selectAll("circle")
      .data(this.data)
      .attr("fill", d => d.orgNonProfit ==="No" ? "#c5a7d9" :"#6dc9a0")
      .transition().duration(500)
    .attr("fill", d => d.orgPublic ==="No" ? "#c5a7d9" : "#db6571")
    }
  }
};
</script>
<style scoped>
circle {
  transition: 0.01s ease-out;
}
svg {
  display: block;
  margin: 0 auto;
  background: none;
}
</style>
