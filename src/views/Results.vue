<template>
  <div class="results">
    <!--<PrintButton />-->
    <div>
      <h1>{{ $t("resultTitle") }}</h1>
      <p>{{ $t("version") }}</p>
    </div>

    <p>
      <a
        class="btn btn-default pull-right"
        role="button"
        :href="$t('linkProjectAnchor')"
      >
        {{ $t("linkProjectText") }}
      </a>
    </p>
    <form>
      <ActionButtonBar
        @fileLoaded="fileLoaded($event)"
        @startAgain="startAgain"
      />
    </form>

    <div class="alert alert-info">
      <p class="small">{{ $t("localSaveWarning") }}</p>
    </div>

    <p>{{ $t("onThisPage") }}</p>
    <ul>
      <li>
        <a href="#score">{{ "Section 1: " + $t("riskLevel") }}</a>
      </li>
      <li>
        <a href="#obligations">{{ $t("requirements.title") }}</a>
      </li>
      <li>
        <a href="#qA">{{ "Section 3: " + $t("resultSectionQA") }}</a>
        <ul>
          <li>
            <a href="#projectDetails">{{
              "Section 3.1: " + $t("resultSectionPD")
            }}</a>
          </li>
          <li>
            <a href="#riskQA">{{ "Section 3.2: " + $t("resultSectionRQA") }}</a>
          </li>
          <li>
            <a href="#mitigationQA">{{
              "Section 3.3: " + $t("resultSectionMQA")
            }}</a>
          </li>
        </ul>
      </li>
    </ul>

    <div id="results-printable">
      <div class="container-fluid">
        <h2 id="score">
          {{ "Section 1: " + $t("riskLevel") }} {{ ": " + score[3] }}
        </h2>
        <h3 style="word-spacing: 5px">
          {{ $t("currentScore") }} {{ ": " + score[2] }}
        </h3>
        <h3>{{ $t("rawRiskScore") }}{{ ": " + score[0] }}</h3>

        <b-table
          striped
          hover
          :items="riskAreaItems()"
          :fields="riskAreaFields"
        ></b-table>

        <h3>{{ $t("mitigationScore") }}{{ ": " + score[1] }}</h3>

        <b-table
          striped
          hover
          :items="mitigationItems()"
          :fields="mitigationFields"
        ></b-table>

        <Obligations />
        <div class="row">
          <h2 id="qA">{{ "Section 3: " + $t("resultSectionQA") }}</h2>
        </div>

        <div class="row">
          <h3 id="projectDetails">
            {{ "Section 3.1: " + $t("resultSectionPD") }}
          </h3>
        </div>
        <div
          class="row"
          v-for="(result, index) in myResults[0]"
          :key="result.name"
        >
          <Result :data="result" :num="index + 1"></Result>
        </div>

        <div class="row">
          <h3 id="riskQA">{{ "Section 3.2: " + $t("resultSectionRQA") }}</h3>
        </div>
        <div
          class="row"
          v-for="(result, index) in myResults[1]"
          :key="result.name"
        >
          <Result :data="result" :num="index + 1"></Result>
        </div>

        <div class="row">
          <h3 id="mitigationQA">
            {{ "Section 3.3: " + $t("resultSectionMQA") }}
          </h3>
        </div>
        <div
          class="row"
          v-for="(result, index) in myResults[2]"
          :key="result.name"
        >
          <Result :data="result" :num="index + 1"></Result>
        </div>
      </div>
    </div>

    <div style="margin-bottom: 15px">
      <h1>{{ $t("export") }}</h1>
      <button
        type="button"
        value="Export Results"
        class="btn btn-default"
        onclick="exportResults('en')"
      >
        {{ $t("exportEnglishResults") }}
      </button>

      <button
        type="button"
        value="Export Results"
        class="btn btn-default"
        onclick="exportResults('fr')"
      >
        {{ $t("exportFrenchResults") }}
      </button>
    </div>

    <details id="en-content-wrap">
      <summary>{{ $t("englishContent") }}</summary>
      <div id="en-content" lang="en">
        <h1>{{ $t("resultTitle", "en") }}</h1>
        <p>{{ $t("version", "en") }}</p>
        <div
          class="row"
          v-for="(result, index) in myResults[0]"
          :key="result.name"
        >
          <Result :data="result" locale="en" :num="index + 1"></Result>
        </div>

        <h2>
          {{ "Section 1: " + $t("riskLevel", "en") }} {{ ": " + score[3] }}
        </h2>
        <div class="pointFormatPDF">
          <p style="word-spacing: 5px">
            {{ $t("currentScore", "en") }}{{ ": " + score[2] }}
          </p>
          <p>{{ $t("rawRiskScore", "en") }}{{ ": " + score[0] }}</p>
          <p>{{ $t("mitigationScore", "en") }}{{ ": " + score[1] }}</p>
        </div>

        <Obligations locale="en" />
        <div class="container-fluid">
          <div class="row">
            <h2 id="qA">{{ "Section 3: " + $t("resultSectionQA", "en") }}</h2>
          </div>

          <div class="row">
            <h3 id="riskQA">
              {{ "Section 3.1: " + $t("resultSectionRQA", "en") }}
            </h3>
          </div>
          <div
            class="row"
            v-for="(result, index) in myResults[1]"
            :key="result.name"
          >
            <Result :data="result" locale="en" :num="index + 1"></Result>
          </div>

          <div class="row">
            <h3 id="mitigationQA">
              {{ "Section 3.2: " + $t("resultSectionMQA", "en") }}
            </h3>
          </div>
          <div
            class="row"
            v-for="(result, index) in myResults[2]"
            :key="result.name"
          >
            <Result :data="result" locale="en" :num="index + 1"></Result>
          </div>
        </div>
      </div>
    </details>

    <details id="fr-content-wrap">
      <summary>{{ $t("frenchContent") }}</summary>
      <div id="fr-content" lang="fr">
        <h1>{{ $t("resultTitle", "fr") }}</h1>
        <p>{{ $t("version", "fr") }}</p>
        <div
          class="row"
          v-for="(result, index) in myResults[0]"
          :key="result.name"
        >
          <Result :data="result" locale="fr" :num="index + 1"></Result>
        </div>

        <h2>
          {{ "Section 1: " + $t("riskLevel", (locale = "fr"))
          }}{{ ": " + score[3] }}
        </h2>

        <div class="pointFormatPDF">
          <p style="word-spacing: 5px">
            {{ $t("currentScore", "fr") }} {{ ": " + score[2] }}
          </p>
          <p>{{ $t("rawRiskScore", "fr") }} {{ ": " + score[0] }}</p>
          <p>{{ $t("mitigationScore", "fr") }} {{ ": " + score[1] }}</p>
        </div>

        <Obligations locale="fr" />

        <div class="container-fluid">
          <div class="row">
            <h2 id="qA">{{ "Section 3: " + $t("resultSectionQA", "fr") }}</h2>
          </div>

          <div class="row">
            <h3 id="riskQA">
              {{ "Section 3.1: " + $t("resultSectionRQA", "fr") }}
            </h3>
          </div>
          <div
            class="row"
            v-for="(result, index) in myResults[1]"
            :key="result.name"
          >
            <Result :data="result" locale="fr" :num="index + 1"></Result>
          </div>

          <div class="row">
            <h3 id="mitigationQA">
              {{ "Section 3.2: " + $t("resultSectionMQA", "fr") }}
            </h3>
          </div>
          <div
            class="row"
            v-for="(result, index) in myResults[2]"
            :key="result.name"
          >
            <Result :data="result" locale="fr" :num="index + 1"></Result>
          </div>
        </div>
      </div>
    </details>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

import { Model } from "survey-vue";

import showdown from "showdown";

import AssessmentTool from "@/components/AssessmentTool.vue"; // @ is an alias to /src
import Score from "@/components/Score.vue";
import ActionButtonBar from "@/components/ActionButtonBar.vue";
import Result from "@/components/Result.vue";
import Obligations from "@/components/Obligations.vue";
import SurveyFile from "@/interfaces/SurveyFile";
import i18n from "@/plugins/i18n";
import surveyJSON from "@/survey-enfr.json";
import RiskArea from "@/interfaces/RiskArea";

@Component({
  components: {
    ActionButtonBar,
    Result,
    Score,
    Obligations
  },
  computed: {
    score: function() {
      return this.$store.getters.calcScore;
    }
  }
})
export default class Results extends Vue {
  myResults = this.$store.getters.resultDataSections;

  riskAreaFields = [
    { key: "risk_area", label: this.$t("riskArea").toString() },
    { key: "no_of_questions", label: this.$t("noOfQuestions").toString() },
    { key: "project_score", label: this.$t("projectScore").toString() },
    { key: "maximum_score", label: this.$t("maximumScore").toString() }
  ];

  mitigationFields = [
    { key: "risk_area", label: this.$t("mitigationArea").toString() },
    { key: "no_of_questions", label: this.$t("noOfQuestions").toString() },
    { key: "project_score", label: this.$t("projectScore").toString() },
    { key: "maximum_score", label: this.$t("maximumScore").toString() }
  ];

  riskAreaItems() {
    let items: RiskArea[] = [];
    let totalNoQuestions = 0;
    let totalProjectScore = 0;
    let totalMaxScore = 0;
    if (this.myResults[1]?.length > 0) {
      this.myResults[1].forEach((myResult: any) => {
        if (myResult.questionHeader !== undefined) {
          let riskAreaTitle = myResult.questionHeader;
          let riskAreaName = myResult.name.replace(/\d/, "");
          let sectionScore = this.$store.getters.getScoreBySection(
            riskAreaName
          );
          items.push({
            risk_area: riskAreaTitle[this.$i18n.locale],
            no_of_questions: sectionScore[0],
            project_score: sectionScore[1],
            maximum_score: sectionScore[2]
          });
          totalNoQuestions += sectionScore[0];
          totalProjectScore += sectionScore[1];
          totalMaxScore += sectionScore[2];
        }
      });
    }
    items.push({
      risk_area: this.$t("rawRiskScore")
        .toString()
        .toUpperCase(),
      no_of_questions: totalNoQuestions,
      project_score: totalProjectScore,
      maximum_score: totalMaxScore
    });
    return items;
  }

  mitigationItems() {
    let items: RiskArea[] = [];
    let totalNoQuestions = 0;
    let totalProjectScore = 0;
    let totalMaxScore = 0;
    if (this.myResults[2]?.length > 0) {
      this.myResults[2].forEach((myResult: any) => {
        if (myResult.questionHeader !== undefined) {
          let riskAreaTitle = myResult.questionHeader;
          let riskAreaName = myResult.name.replace(/\d/, "");
          let sectionScore = this.$store.getters.getScoreBySection(
            riskAreaName
          );
          items.push({
            risk_area: riskAreaTitle[this.$i18n.locale]
              ? riskAreaTitle[this.$i18n.locale]
              : riskAreaTitle["en"],
            no_of_questions: sectionScore[0],
            project_score: sectionScore[1],
            maximum_score: sectionScore[2]
          });
          totalNoQuestions += sectionScore[0];
          totalProjectScore += sectionScore[1];
          totalMaxScore += sectionScore[2];
        }
      });
    }
    items.push({
      risk_area: this.$t("mitigationScore")
        .toString()
        .toUpperCase(),
      no_of_questions: totalNoQuestions,
      project_score: totalProjectScore,
      maximum_score: totalMaxScore
    });
    return items;
  }

  Survey: Model = new Model(surveyJSON);

  startAgain() {
    this.Survey.clear(true, true);
    window.localStorage.clear();
    this.$store.commit("resetSurvey");
    this.$router.push({ path: "/" });
  }
  fileLoaded($event: SurveyFile) {
    this.Survey.version = $event.version;
    this.Survey.data = $event.data;
    this.Survey.translationsOnResult = $event.translationsOnResult;
    this.Survey.currentPageNo = $event.currentPage;
    this.Survey.start();
    this.$store.commit("updateResult", this.Survey);

    this.myResults = this.$store.getters.resultDataSections;
  }

  created() {
    this.Survey.onComplete.add(result => {
      this.$store.commit("updateResult", result);
    });

    this.Survey.onComplete.add(results => {
      this.$router.push("Results");
    });

    this.Survey.onValueChanged.add(result => {
      this.$store.commit("updateResult", result);
    });

    const converter = new showdown.Converter();

    this.Survey.onTextMarkdown.add(function(survey, options) {
      //convert the markdown text to html
      var str = converter.makeHtml(options.text);
      //remove root paragraphs <p></p>
      str = str.substring(3);
      str = str.substring(0, str.length - 4);
      //set html
      options.html = str;
    });

    // Set locale
    this.Survey.locale = i18n.locale;

    // Remove the default required '*'.
    this.Survey.requiredText = "";

    // Fix all the question labels as they're using <H5> instead of <label>
    // as SurveyJS has open issue as per: https://github.com/surveyjs/surveyjs/issues/928
    this.Survey.onAfterRenderQuestion.add(function(sender, options) {
      let title = options.htmlElement.getElementsByTagName("H5")[0];
      if (title) {
        var questionRequiredHTML = "";

        if (options.question.isRequired) {
          // Should do localization mechanism
          var requiredText = sender.locale == "fr" ? "obligatoire" : "required";
          questionRequiredHTML =
            ' <strong class="required">(' + requiredText + ")</strong>";
        }

        title.outerHTML =
          '<label for="' +
          options.question.inputId +
          '" class="' +
          title.className +
          '"><span class="field-name">' +
          title.innerText +
          "</span>" +
          questionRequiredHTML +
          "</label>";
      }
    });

    //if survey is in progress reload from store
    if (this.$store.getters.inProgress) {
      this.fileLoaded({
        version: this.$store.state.version,
        currentPage: this.$store.state.currentPageNo,
        data: this.$store.state.toolData,
        translationsOnResult: this.$store.state.translationsOnResult
      } as SurveyFile);
    }
  }
}
</script>
