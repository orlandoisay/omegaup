<template>
  <div class="container-lg p-5 w-50">
    <div class="card">
      <h2 class="card-header">{{ T.passwordResetRequestTitle }}</h2>
      <form class="card-body">
        <div class="form-group">
          <h5 class="card-title">{{ T.profileEmail }}</h5>
          <input
            type="text"
            name="email"
            class="form-control"
            v-model="email"
          />
        </div>
        <button
          class="btn btn-primary form-control"
          v-on:click.prevent="$emit('forgot-password', email)"
        >
          {{ T.wordsSend }}
        </button>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';
import { omegaup } from '../../omegaup';
import T from '../../lang';

@Component
export default class PasswordRecover extends Vue {
  T = T;
  email: string = '';
}
</script>
