<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <List
        :list="items"
        :finalArray="finalArray"
        :key="finalArray.priority"
        @append:priority="appendPriority"
        @update:selected="updateSelected"
        @sort:selected="sortSelected"
        @delete:selected="deleteSelected"
        @submit:properties="submit"
      />
    </v-col>
  </v-row>
</template>

<script>
import List from "../components/List";

export default {
  name: "IndexPage",
  components: {
    List,
  },

  mounted() {
    this.items.forEach((item, index) => {
      item.index = index;
      item.order = item.orderTypeDefault;
      item.disabled = false;
    });
  },
  data() {
    return {
      currentPriority: 0,
      finalArray: [],
      items: [
        {
          name: "affiliate",
          title: "Affiliate",
          orderTypeDefault: "ASC",
          priority: 0,
        },
        {
          name: "balance",
          title: "Balance",
          orderTypeDefault: "DESC",
          priority: 0,
        },
        {
          name: "bonus_balance",
          title: "Bonus balance",
          orderTypeDefault: "DESC",
          priority: 0,
        },
        {
          name: "campaign",
          title: "Campaign",
          orderTypeDefault: "ASC",
          priority: 0,
        },
        {
          name: "cash_balance",
          title: "Cash balance",
          orderTypeDefault: "DESC",
          priority: 0,
        },
        {
          name: "country",
          title: "Country",
          orderTypeDefault: "ASC",
          priority: 0,
        },
        {
          name: "trader_points",
          title: "Trader points",
          orderTypeDefault: "DESC",
          priority: 0,
        },
      ],
    };
  },
  methods: {
    appendPriority(property) {
      this.finalArray.push(property);
      this.currentPriority += 1;
      this.finalArray[this.finalArray.length - 1].priority =
        this.currentPriority;
    },
    updateSelected(selected) {
      const foundIndex = this.items.findIndex((element) => {
        return element.name === selected.name ? true : false;
      });

      this.items[foundIndex].disabled = true;
      let current = this.items.filter((item) => item.name === selected.name)[0];
      this.finalArray[this.finalArray.length - 1].order =
        current && current.order ? current.order : "DESC";

      this.finalArray[this.finalArray.length - 1].property =
        current && current.name ? current.name : "";
    },
    deleteSelected(selected) {
      // find selected in options
      this.$nextTick(() => {
        let current = this.items.filter(
          (item) => item.name === selected.property
        )[0];
        current.disabled = false;

        // Identify and delete item by index
        let foundIndex = this.finalArray.findIndex((item) => {
          return item.property === current.name;
        });
        this.finalArray.splice(foundIndex, 1);

        // fix priority and numeration
        this.finalArray.forEach((item) => {
          if (item.priority > foundIndex) item.priority--;
        });
        this.currentPriority -= 1;
      });
    },
    sortSelected(selected) {
      // find selected in options
      let current = this.items.filter(
        (item) => item.name === selected.property
      )[0];
      let foundIndex = this.finalArray.findIndex((item) => {
        return item.property === current.name;
      });

      if (
        this.finalArray[foundIndex] &&
        this.finalArray[foundIndex].order === "ASC"
      ) {
        this.finalArray[foundIndex].order = "DESC";
      } else {
        this.finalArray[foundIndex].order = "ASC";
      }
    },
    submit() {
      console.log("FINAL LIST: ", this.finalArray);
      return this.finalArray;
    },
  },
  watch: {
    items: {
      deep: true,
      handler: function (newValue, oldValue) {},
    },
  },
};
</script>
