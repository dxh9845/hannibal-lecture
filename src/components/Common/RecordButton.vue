<template>
    <b-btn @click="toggle" variant="light" size="sm" class="my-sm-0" :pressed="recording">
        <span class='mr-1'>{{ icon }}</span> {{ text }}
    </b-btn>
</template>

<script>
import ScriptProcessor from '@/services/ScriptProcessor';

export default {
  name: 'record-button',
  data() {
    return {
      recording: false,
    };
  },
  computed: {
    icon() {
      return this.recording ? '⬛' : '🔴';
    },
    text() {
      return this.recording ? 'Stop recording' : 'Start recording';
    },
  },
  mounted() {
    this.sp = new ScriptProcessor(this.$socket);
  },
  methods: {
    async toggle() {
      switch (this.recording) {
        case false:
          await this.askRecording();
          break;
        case true:
          this.stopRecording();
          break;
        default:
          break;
      }
      this.recording = !this.recording;
    },
    async askRecording() {
      try {
        await this.sp.startRecording();
      } catch (error) {
        console.error('Failed to start recording.');
      }
    },
    stopRecording() {
      this.sp.stopRecording();
    },
  },
};
</script>
