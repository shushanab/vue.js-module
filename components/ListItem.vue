<!-- Please remove this file from your project -->
<template>
  <v-card class="mt-4">
    <v-card-text>
      <v-layout row>
        <v-col cols="1">
          <p class="pt-2" v-if="item.priority">
            {{ item.priority }}
          </p>
        </v-col>
        <v-col cols="7">
          <v-row>
            <v-autocomplete
              v-model="selected"
              item-value="name"
              item-text="title"
              return-object
              append-icon="uil-angle-down"
              placeholder="Select"
              :items="list"
              :disabled="selected ? true : false"
              @change="selectProperty"
            ></v-autocomplete>
          </v-row>
          <v-spacer></v-spacer>
        </v-col>
        <v-col cols="4" class="text-right">
          <v-btn icon @click="sortProperty(item)" :disabled="disableButtons">
            <v-tooltip top>
              <template #activator="{ on }">
                <v-icon v-on="on" color="primary">{{
                  item.order === "ASC"
                    ? "uil-sort-amount-down"
                    : "uil-sort-amount-up"
                }}</v-icon>
              </template>
              <span>
                {{
                  item.order === "ASC" ? "Sort Descending" : "Sort Assending"
                }}
              </span>
            </v-tooltip>
          </v-btn>

          <v-btn icon @click="deleteProperty(item)" :disabled="disableButtons">
            <v-tooltip top>
              <template #activator="{ on }">
                <v-icon v-on="on" color="error">uil-trash</v-icon>
              </template>
              <span> Delete Property </span></v-tooltip
            >
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
    disableButtons: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    selectProperty(e) {
      this.$emit("selected:property", e);
    },
    sortProperty(e) {
      this.$emit("sort:property", e);
    },
    deleteProperty(e) {
      this.$emit("delete:property", e);
    },
  },
};
</script>
