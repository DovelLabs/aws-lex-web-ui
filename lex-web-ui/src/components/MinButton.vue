<template>
  <v-container fluid class="min-button-container">
    <v-tooltip
      v-model="shouldShowTooltip"
      content-class="tooltip-min-button"
      activator=".min-button"
      left
    >
      <span id="min-button-tooltip">{{minButtonToolTipContent}}</span>
    </v-tooltip>
    <v-fab-transition>
      <v-btn
        bottom
        dark
        depressed
        fab
        fixed
        right
        v-show="isUiMinimized"
        v-bind:color="toolbarColor"
        v-on:click.stop="toggleMinimize"
        v-on="tooltipEventHandlers"
        aria-label="show chat window"
        class="min-button"
      >
        <v-icon>
          {{'chat'}}
        </v-icon>
      </v-btn>
    </v-fab-transition>
  </v-container>
</template>

<script>
/*
Copyright 2017-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Licensed under the Amazon Software License (the "License"). You may not use this file
except in compliance with the License. A copy of the License is located at

http://aws.amazon.com/asl/

or in the "license" file accompanying this file. This file is distributed on an "AS IS"
BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, express or implied. See the
License for the specific language governing permissions and limitations under the License.
*/
export default {
  name: 'min-button',
  data() {
    return {
      shouldShowTooltip: false,
      tooltipEventHandlers: {
        mouseenter: this.onInputButtonHoverEnter,
        mouseleave: this.onInputButtonHoverLeave,
        touchstart: this.onInputButtonHoverEnter,
        touchend: this.onInputButtonHoverLeave,
        touchcancel: this.onInputButtonHoverLeave,
      },
    };
  },
  props: ['toolbarColor', 'isUiMinimized'],
  computed: {
    toolTipMinimize() {
      return (this.isUiMinimized) ? 'maximize' : 'minimize';
    },
    minButtonToolTipContent() {
      return this.$store.state.config.ui.minButtonToolTipContent;
    },
  },
  methods: {
    onInputButtonHoverEnter() {
      this.shouldShowTooltip = true;
    },
    onInputButtonHoverLeave() {
      this.shouldShowTooltip = false;
    },
    toggleMinimize() {
      if (this.$store.state.isRunningEmbedded) {
        this.onInputButtonHoverLeave();
        this.$emit('toggleMinimizeUi');
      }
    },
  },
};
</script>
<style>
</style>
