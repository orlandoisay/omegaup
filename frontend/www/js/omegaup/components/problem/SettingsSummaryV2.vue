<template>
  <div>
    <h3 class="text-center mb-4">
      {{ title }}
      <template v-if="showVisibilityIndicators">
        <img
          src="/media/quality-badge-sm.png"
          v-bind:title="T.wordsHighQualityProblem"
          v-if="problem.quality_seal || problem.visibility === 3"
          class="mr-2"
        />
        <font-awesome-icon
          v-bind:icon="['fas', 'exclamation-triangle']"
          v-bind:title="T.wordsWarningProblem"
          v-if="problem.visibility === 1 || problem.visibility === -1"
          class="mr-2"
        ></font-awesome-icon>
        <font-awesome-icon
          v-bind:icon="['fas', 'eye-slash']"
          v-bind:title="T.wordsPrivate"
          v-if="problem.visibility === 0 || problem.visibility === -1"
          class="mr-2"
        ></font-awesome-icon>
        <font-awesome-icon
          v-bind:icon="['fas', 'ban']"
          v-bind:title="T.wordsBannedProblem"
          v-if="problem.visibility <= -2"
          class="mr-2"
          color="darkred"
        ></font-awesome-icon>
      </template>

      <a v-if="showEditLink" v-bind:href="`/problem/${problem.alias}/edit/`">
        <font-awesome-icon v-bind:icon="['fas', 'edit']" />
      </a>
    </h3>
    <table
      v-if="problem.accepts_submissions"
      class="table table-bordered mx-auto w-75 mb-0"
    >
      <tr>
        <th scope="row">{{ T.wordsPoints }}</th>
        <td>{{ problem.points }}</td>
        <th scope="row">{{ T.arenaCommonMemoryLimit }}</th>
        <td data-memory-limit>{{ memoryLimit }}</td>
      </tr>
      <tr>
        <th scope="row">{{ T.arenaCommonTimeLimit }}</th>
        <td>{{ timeLimit }}</td>
        <th scope="row">{{ T.arenaCommonOverallWallTimeLimit }}</th>
        <td>{{ overallWallTimeLimit }}</td>
      </tr>
      <tr>
        <template v-if="!showVisibilityIndicators">
          <th scope="row">{{ T.wordsInOut }}</th>
          <td>{{ T.wordsConsole }}</td>
        </template>
        <th scope="row">{{ T.problemEditFormInputLimit }}</th>
        <td>{{ inputLimit }}</td>
      </tr>
    </table>
  </div>
</template>

<style lang="scss" scoped>
@import '../../../../sass/main.scss';

table td {
  padding: 0.5rem;
}
</style>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import T from '../../lang';
import * as ui from '../../ui';
import { types } from '../../api_types';

import { library } from '@fortawesome/fontawesome-svg-core';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import {
  faEdit,
  faExclamationTriangle,
  faEyeSlash,
  faBan,
  faExternalLinkAlt,
} from '@fortawesome/free-solid-svg-icons';
library.add(
  faExclamationTriangle,
  faEdit,
  faEyeSlash,
  faBan,
  faExternalLinkAlt,
);

@Component({
  components: {
    FontAwesomeIcon,
  },
})
export default class ProblemSettingsSummary extends Vue {
  @Prop() problem!: types.ArenaProblemDetails;
  @Prop({ default: false }) showVisibilityIndicators!: boolean;
  @Prop({ default: false }) showEditLink!: boolean;

  T = T;

  get title(): string {
    if (this.showVisibilityIndicators) {
      return `${this.problem.problem_id}. ${this.problem.title}`;
    }
    if (!this.problem.letter) {
      return this.problem.title;
    }
    return `${this.problem.letter}. ${this.problem.title}`;
  }

  get memoryLimit(): string {
    if (!this.problem.settings?.limits.MemoryLimit) {
      return '';
    }
    if (typeof this.problem.settings?.limits.MemoryLimit === 'string') {
      return this.problem.settings?.limits.MemoryLimit;
    }
    const memoryLimit = this.problem.settings?.limits.MemoryLimit as number;
    return `${memoryLimit / 1024 / 1024} MiB`;
  }

  get timeLimit(): string {
    if (!this.problem.settings?.limits.TimeLimit) {
      return '';
    }
    return `${this.problem.settings?.limits.TimeLimit}`;
  }

  get overallWallTimeLimit(): string {
    if (!this.problem.settings?.limits.OverallWallTimeLimit) {
      return '';
    }
    return `${this.problem.settings?.limits.OverallWallTimeLimit}`;
  }

  get inputLimit(): string {
    if (!this.problem.input_limit) {
      return '';
    }
    return `${this.problem.input_limit / 1024} KiB`;
  }
}
</script>
