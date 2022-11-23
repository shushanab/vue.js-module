<!-- Please remove this file from your project -->
<template>
  <v-card class="mt-4" v-if="item.priority !== 0">
    <v-card-text>
      <v-layout row>
        <v-col cols="1">
          <strong>
            {{ item.priority }}
          </strong>
        </v-col>
        <v-col cols="8">
          <v-row>
            <!--{{ selected }}-->
            <!--
              :v-model="selected ? selected : ''"
              required
              -->
            <v-autocomplete
              v-model="selected"
              item-value="name"
              item-text="title"
              return-object
              append-icon="uil-angle-down"
              placeholder="Select"
              :items="list"
              @change="selectProperty"
            ></v-autocomplete>
            <!--{{ item }}-->
          </v-row>
          <v-spacer></v-spacer>
        </v-col>
        <v-col cols="3" class="text-right">
          {{ item.priority }}
          <v-btn
            icon
            @click="sortProperty"
            :disabled="item.priority === 0 ? true : false"
          >
            <v-icon color="primary">{{
              item.order === "ASC"
                ? "uil-sort-amount-down"
                : "uil-sort-amount-up"
            }}</v-icon>
          </v-btn>
          <v-btn icon @click="deleteProperty">
            <v-icon color="error">uil-trash</v-icon>
          </v-btn>
        </v-col>
      </v-layout>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      selected: null,
    };
  },
  props: {
    item: {
      type: Object,
      default: () => {},
    },
    list: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    selectProperty(e) {
      this.$emit("selected:property", e.name);
    },
    sortProperty(e) {
      this.$emit("sort:property", e.name);
    },
    deleteProperty(e) {
      this.$emit("deleted:property", e.name);
    },
  },
};
</script>
