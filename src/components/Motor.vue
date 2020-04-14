<template>
  <v-card>
    <v-card-title class="headline">Motor {{ id }}</v-card-title>
    <v-card-subtitle>
      <v-text-field
        label="Resource ID:"
        :id="'motor-'+ id"
        v-model="resource"
        @input="$emit('updateResource', resource)"
        :rules="resourceRule"
      ></v-text-field>
      <v-select
        label="Which motor spins?"
        v-model="motor"
        @input="$emit('updateMotor', motor)"
        :rules="[v => !!v || 'Motor is required']"
        :items="items"
        :id="'motor-' + id + '-spins'"
      ></v-select>
    </v-card-subtitle>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      items: [
        'Motor 1',
        'Motor 2',
        'Motor 3',
        'Motor 4',
      ],
      resourceRule: [
        (v) => !!v || 'Resource ID is required',
      ],
    };
  },
  props: {
    id: Number,
    parentData: Array,
  },
};
</script>

<style lang="scss" scoped>
  .padding-1 {
    padding: 1rem;
  }
</style>
