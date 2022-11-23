<template>
  <v-card elevation="0" color="#fafafa" class="ma-2 pa-2">
    {{ emptyModel }}
    -----
    {{ list }}
    <v-card-text>
      <ListItem
        v-show="isNew"
        :item="emptyModel"
        :list="list"
        @selected:property="selectedPropertyFromChild"
      />

      <div v-for="(item, index) in list">
        <ListItem
          :item="item"
          :list="list"
          @selected:property="selectedPropertyFromChild"
          @sort:property="sortPropertyFromChild"
          @delete:property="deletePropertyFromChild"
        />
      </div>

      <v-divider v-if="isNew" />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn
        color="primary"
        outlined
        @click="addProperty"
        :disabled="isDisabled"
      >
        <v-icon>uil-plus</v-icon>
        Add property
      </v-btn>
      <!--TODO add tooltip here -->
      <v-btn color="primary" @click="submit"> Submit </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      isNew: false,
      emptyModel: {
        name: "",
        title: "",
        orderTypeDefault: "DESC",
        priority: 1,
      },
    };
  },
  props: {
    list: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    addProperty() {
      this.isNew = true;
    },
    selectedPropertyFromChild(name) {
      //if (this.isNew) {
      this.isNew = false;
      //}
      this.$emit("update:selected", name);
    },
    sortPropertyFromChild(name) {
      this.$emit("sort:selected", name);
    },
    deletePropertyFromChild(name) {
      this.$emit("delete:selected", name);
    },
    submit() {
      const arr = this.list.map(this.formatItem);
      console.log("FINAL LIST: ", arr);
    },
    formatItem(item) {
      return {
        order: item.order,
        property: item.name,
        priority: item.priority,
      };
    },
  },
  computed: {
    isDisabled() {
      return this.list.every((item) => item.priority != 0);
    },
  },
};
</script>
