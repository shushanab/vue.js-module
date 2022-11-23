<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <List
        :list="items"
        @update:selected="updateSelected"
        @sort:selected="sortSelected"
        @delete:selected="deleteSelected"
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
    updateSelected(name) {
      const foundIndex = this.items.findIndex((element) => {
        return element.name === name ? true : false;
      });
      console.log("Selected", name, foundIndex);
      this.currentPriority += 1;
      this.items[foundIndex].priority = this.currentPriority;
      this.items[foundIndex].disabled = true;
    },
    deleteSelected(name) {
      const foundIndex = this.items.findIndex((element) => {
        return element.name === name ? true : false;
      });
      console.log("Delete", name, foundIndex);
      this.currentPriority -= 1;

      this.items[foundIndex].priority = 0;
      this.items[foundIndex].disabled = false;
    },
    sortSelected(name) {
      const foundIndex = this.items.findIndex((element) => {
        return element.name === name ? true : false;
      });
      console.log("Sort", name, foundIndex);
      if (this.items[foundIndex] && this.items[foundIndex].order === "ASC") {
        this.items[foundIndex].order = "DESC";
      } else {
        this.items[foundIndex].order = "ASC";
      }
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
