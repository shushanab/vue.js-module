<template>
  <v-card elevation="0" color="#fafafa" class="ma-2 pa-2">
    <v-card-text>
      <h3>
        <v-icon>uil-info-circle</v-icon>Add, Select, Sort or Delete some
        Properties
      </h3>

      <p v-if="!finalArray.length" class="text-center mt-8">
        There is no properties created yet, please add
      </p>
      <div v-else>
        <v-layout row class="mt-4" v-if="finalArray">
          <v-col cols="1"><h4 class="pl-4">N</h4></v-col>
          <v-col cols="7"> <h4>Property</h4></v-col>
          <v-col cols="4" class="text-right"
            ><v-spacer></v-spacer>
            <h4 class="pr-4">Actions</h4>
          </v-col>
        </v-layout>
        <div v-for="(item, index) in finalArray">
          <ListItem
            :item="item"
            :list="list"
            :key="item.priority"
            :disableButtons="disableButtons"
            @selected:property="selectedPropertyFromChild"
            @sort:property="sortPropertyFromChild"
            @delete:property="deletePropertyFromChild"
          />
        </div>
      </div>
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn
        color="primary"
        outlined
        @click="addProperty"
        :disabled="isDisabled || disableButtons"
      >
        <v-icon color="primary">uil-plus</v-icon>
        Add property
      </v-btn>
      <v-btn :disabled="disableButtons" color="primary" @click="submit">
        Submit
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      disableButtons: false,
      newProperty: {
        priority: 1,
        property: "",
        order: "DESC",
      },
    };
  },
  props: {
    list: {
      type: Array,
      default: () => [],
    },
    finalArray: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    addProperty() {
      let temp = Object.assign({}, this.newProperty);

      this.$emit("append:priority", temp);
      this.$nextTick(() => {
        if (
          this.finalArray[this.finalArray.length - 1] &&
          this.finalArray[this.finalArray.length - 1].property === ""
        )
          this.disableButtons = true;
      });
    },
    selectedPropertyFromChild(e) {
      this.disableButtons = false;
      this.$emit("update:selected", e);
    },
    sortPropertyFromChild(e) {
      this.$emit("sort:selected", e);
    },
    deletePropertyFromChild(e) {
      this.$emit("delete:selected", e);
    },
    submit() {
      this.$emit("submit:properties");
    },
  },
  computed: {
    isDisabled() {
      return this.list.length == this.finalArray.length;
    },
  },
};
</script>
