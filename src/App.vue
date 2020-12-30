<template>
  <div id="app">
    <div class="intro">
      <h1>
        Members of the data visualization community as 100 people
      </h1>
      <p class="byline">
        by
        <a href="https://twitter.com/mashfedorova" target="_blank"
          >Maria Fedorova</a
        >
      </p>
      <p class="p-intro">
        At the end of 2020
        <a href="https://www.datavisualizationsociety.com/" target="_blank">
          Data visualization Society</a
        >
        published
        <a
          href="https://github.com/data-visualization-society/data_visualization_survey/tree/master/data/2020"
          target="_blank"
          >data</a
        >
        from its "Annual Census" - an extensive survey of data visualization
        practitioners. The survey included responses from 1,766 people. As a newcomer
        to the field myself, I wanted to dive into the data to understand who
        are the people doing visualization in terms of demographics, where do they
        work and what do they study.
      </p>
    </div>
    <div id="chart">
      <force-overview> </force-overview>
    </div>
    <article id="scroll-steps">
      <section class="step" id="step1">
        Let`s look at 100 dots, with one dot representing one per cent of all
        respondents — approximately 18 people.
      </section>
      <section class="step" id="step2">
        Slightly more than half of all people who answered the survey live in
        either the United States, the United Kingdom, or Canada.
      </section>
      <section class="step" id="step3">
        The other half of respondents reported that they live in approximately
        80 different countries from all over the world:
        <span class="europe">Europe</span>,
        <span class="asia">Asia and Oceania</span>,
        <span class="latAm"> Latin America and the Caribbean</span>,
       and <span class ="africa">Africa</span>.
      </section>
      <section class="step" id="step4">
        Most of the respondents are relatively new in the data visualization world.
        Approximately one in seven respondents said that they have been doing data
        visualization for 5 years or less.
      </section>
      <section class="step" id="step5">
        Around half of all respondents reported having obtained a Master degree
        (or its equivalent) and 15 per cent reported having obtained a PhD.
      </section>
      <section class="step" id="step6">
        Degrees in science, math and technology are  the most popular ones
        among the data visualization practitioners, followed by degrees in arts
        and humanities, and social sciences.
      </section>
      <section class="step" id="step7">
        15% of all respondents are working in the <span class="private"> private sector. </span> Those who majored
        in business or science, math and technology ar more likely than respondents
        who pursued other degrees to work in the private sector.
      </section>
      <section class="step" id="step8">
        One in ten respondents is working in the <span class="public">public sector. </span> Respondents
        who specialized in social sciences are more likely than respondents who hold
        other degrees to work in the public sector.
      </section>
      <section class="step" id="step9">
        One in ten respondents is working in
        <span class="nonProfit"> non-profit sector.</span> Those respondents who
        have degrees in arts and humanities, as well as in social sciences are
        slightly more likely to work in non-profit sector.
      </section>
      <section class="step" id="step10">
    Overall, half of respondents identified as <span class="men"> men, </span> while 40% identified
        <span class="women"> women, </span> and 8 per cent identified as
        <span class="other"> non-binary. </span> There are twice as many men as
        women who had a business degree, as well as science, math and technology
        degree. The gender distribution is more equal among respondents who had
        degrees in social sciences and arts and humanities.
        </section>
  <section class="step" id="step11">
    The vast majority of all respondents learnt data visualization by themselves, without
    attending a specialized education in data visualization.
        </section>
  <section class="step" id="step12">
      The largest share of respondents reported an annual salary ranging between
      60,000 and 70,000 USD.
      </section>
      <section class="step" id="step13">
        While <span class="men"> men </span> and
        <span class="women"> women </span> reported approximately similar
        salaries, the share of men was higher among those respondents who
        reported an annual salary of more than 120,000 USD.
      </section>
      <section class="step" id="step14">
        Around 30 per cent of respondents who reported their annual salary between
        100,000 and 140,00 USD said that data visualization was the <span class="main"> main focus </span>
        of their job, a slightly higher share than those who said data
        visualization was their
        <span class="secondary"> secondary </span> focus or <span class="neither">
       neither primary or secondary  </span> focus.
      </section>
    </article>
    <h3> Notes </h3>
    <p class="end-notes">
      Education degrees are recoded as follows: in a person finished Master
      degree, specialization for a Master degree was used, but if a person only
      finished Bachelor degree, specialization for the Bachelor degree was
      taken. <br />
      Made with D3.js, Greensock and Vue.js.
    </p>
  </div>
</template>

<script>
import ForceOverview from "./components/ForceOverview.vue";

export default {
  components: {
    ForceOverview
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Merriweather:wght@700&family=Montserrat&display=swap');

body {
  margin: 0 auto;
  width: 100%;
  font-size: 1 rem;
  font-family: 'Montserrat', sans-serif;
}

h1 {
  margin: 0 auto;
  width: 50%;
  font-family: 'Merriweather', serif;
  color: whitesmoke;
  }
.p-intro {
  margin: 0 auto;
  width: 80%;
  padding-top: 3em;
  line-height: 1.7;
  color: whitesmoke;
}
.intro {
  height: 90vh;
  background: #b38ace;
  padding: 18em 0 0 0;
  text-align: center;
}
.step {
  border: 1px solid #969696;;
  margin: 0 auto;
  width: 55%;
  border-radius: 5px;
  margin-bottom: 85vh;
  margin-top: 20vh;
  position: relative;
  background-color: rgba(255, 255, 255, 0.7);
  padding: 1em;
  line-height: 1.4;
  color: #414141;
  font-weight: bold;

}
 p {
margin:0;
 }
.europe {
  color: #7ea8bf;
}
.latAm {
  color: #6dc9a0;
}
.africa {
  color: #dbb516;
}
.asia {
  color: #db6571;
}
.end-notes {
  padding: 0 10em 30em 10em;
  text-align: center;
  font-size: 0.7em
}

h3 {
  text-align: center;
  font-size: 1em
  }

.private {
  color:#dbb516;
}
.public {
  color: #db6571;
}
.nonProfit {
  color: #6dc9a0;
}
.men {
  color:#6dc9a0;
}
.women {
  color:#db6571;
  }
.other {
  color:#dbb516;
  }
  .main {
    color: #9660BA;
  }
  .secondary {
    color: #c5a7d9;
  }
  .neither {
    color: gray;
  }
  .byline {
    color: whitesmoke;
    padding-top: 1em;
  }
  a:link {
    color:whitesmoke;
  }
  a:hover {
    color: #3b2a46;
  }
  a:focus {
    color: #3b2a46;
    }

</style>
