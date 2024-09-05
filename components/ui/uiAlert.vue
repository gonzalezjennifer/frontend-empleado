<template>
  <v-alert :color="color" :type="type">
    <v-icon>
      {{ icon }}
    </v-icon>
    {{ message }}
  </v-alert>
</template>

<script>
export default {
  props: {
    color: { type: String, default: 'green' },
    type: { type: String, default: 'success' },
    icon: { type: String, default: '' },
    message: { type: String, default: '' }
  }
}
</script>
