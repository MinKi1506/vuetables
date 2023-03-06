<template>
  <div class="ui container">
    <vuetable
      ref="vuetable"
      api-url="https://vuetable.ratiw.net/api/users"
      :fields="fields"
    ></vuetable>
    <vuetable-pagination ref="pagination"></vuetable-pagination>
  </div>
</template>

<script>
import Vuetable from "vuetable-2/src/components/Vuetable";
import VuetablePagination from "vuetable-2/src/components/VuetablePagination";
import accounting from "accounting";
import VuetablePagination from "vuetable-2/src/components/VuetablePaginationDropdown";
import moment from "moment";

export default {
  components: {
    Vuetable,
    VuetablePagination,
  },
  data() {
    return {
      fields: [
        { name: "nickname", titleClass: "center aligned", callback: "allcap" },
        {
          name: "gender",
          titleClass: "center aligned",
          dataClass: "center aligned",
          callback: "genderLabel",
        },
        {
          name: "salary",
          titleClass: "center aligned",
          dataClass: "right aligned",
          callback: "formatNumber",
        },
        {
          name: "birthdate",
          titleClass: "center aligned",
          dataClass: "right aligned",
          callback: "formatDate|YYYY-MM-DD",
        },
      ],
    };
  },
  methods: {
    allcap(value) {
      return value.toUpperCase();
    },
    genderLabel(value) {
      return value === "M"
        ? '<span class="ui teal label"><i class="large man icon"></i>Male</span>'
        : '<span class="ui pink label"><i class="large woman icon"></i>Female</span>';
    },
    formatNumber(value) {
      return accounting.formatNumber(value, 2);
    },
    formatData(value, fmt = "DD MM YYYY") {
      return value == null ? "" : moment(value, "YYYY-MM-DD").format(fmt);
    },
    onPaginationData(paginationData) {
      this.$refs.pagination.setPaginationData(paginationData);
    },
    onChangePage(page) {
      this.$refs.vuetable.changePage(page);
    },
  },
};
</script>

<style></style>
