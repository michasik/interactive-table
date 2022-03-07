<template>
  <div style="display: flex">
    <table>
      <!-- MAIN HEADER ROW -->
      <tr>
        <th click>🔎</th>
        <template v-for="frontend in selectedData.data" :key="frontend.name">
          <th class="selected" @click="removeSelection(frontend.id)">
            ✅ {{ frontend.name }}
          </th>
        </template>

        <template v-for="frontend in tableData.data" :key="frontend.name">
          <th
            v-if="!Object.keys(selectedData.data).includes(frontend.id)"
            @click="addSelection(frontend.id)"
          >
            ⬜ {{ frontend.name }}
          </th>
        </template>
      </tr>

      <!-- TABLE BODY -->
      <template v-for="(groupKey, index) in Object.keys(tableData.attrs)">
        <!-- TABLE INNER HEADERS -->
        <tr
          v-if="
            tableData.attrs[groupKey].header &&
            Object.keys(selectedAttrs[groupKey]).length > 2
          "
          :key="index + tableData.attrs[groupKey].header"
        >
          <td colspan="100%">{{ tableData.attrs[groupKey].header }}</td>
        </tr>

        <!-- VERTICAL HEADERS -->
        <template
          v-for="attr in Object.keys(selectedAttrs[groupKey])"
          :key="attr.toString()"
        >
          <tr>
            <th v-if="!['header', 'name'].includes(attr)">
              {{ tableData.attrs[groupKey][attr] }}
            </th>

            <!-- ACTUAL DATA -->
            <template v-for="data in selectedData.data">
              <td v-if="!['header', 'name'].includes(attr)" class="selected">
                {{ data[groupKey][attr] }}
              </td>
            </template>
            <template v-for="data in tableData.data">
              <td
                v-if="
                  !['header', 'name'].includes(attr) &&
                  !Object.keys(selectedData.data).includes(data.id)
                "
              >
                {{ data[groupKey][attr] }}
              </td>
            </template>
          </tr>
        </template>
      </template>
    </table>

    <!-- FILTERS -->
    <div style="margin: 2rem">
      <div
        v-for="attrGroupKey in Object.keys(tableData.attrs)"
        :key="tableData.attrs[attrGroupKey].name"
        style="margin: 1rem 0"
      >
        {{ tableData.attrs[attrGroupKey].name }}:
        <template
          v-for="attrKey in Object.keys(tableData.attrs[attrGroupKey])"
          :key="tableData.attrs[attrGroupKey].name + '_' + attrKey"
        >
          <button
            v-if="!['header', 'name'].includes(attrKey)"
            @click="toggleSelectedAttrs(attrGroupKey, attrKey)"
            style="margin: 0.25rem"
          >
            {{
              selectedAttrs[attrGroupKey] &&
              selectedAttrs[attrGroupKey][attrKey]
                ? "✅"
                : "⬜"
            }}
            {{ tableData.attrs[attrGroupKey][attrKey] }}
          </button>
        </template>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";

// INITIAL API DATA MOCK
const tableData = ref({
  data: {
    vsf1: {
      id: "vsf1", // todo: remove?
      name: "VSF 1",
      community: {
        openSource: true,
        isGood: "lol",
        githubStars: 4323,
      },
      eCommerce: {
        magento: true,
        shopify: true,
        commercetools: "paid",
        shopware: true,
      },
      cmsIntegarion: {
        storyblok: true,
        prepr: false,
      },
    },
    vsfNext: {
      id: "vsfNext",
      name: "VSF Next",
      community: {
        openSource: true,
        isGood: "yeah but nah",
        // githubStars: 2345,
      },
      eCommerce: {
        magento: true,
        shopify: true,
        commercetools: "paid",
        shopware: false,
      },
      cmsIntegarion: {
        storyblok: true,
        prepr: false,
      },
    },
    custom: {
      id: "custom",
      name: "Custom",
      community: {
        openSource: false,
        isGood: "might be",
        githubStars: "---",
      },
      eCommerce: {
        magento: true,
        shopify: true,
        commercetools: true,
        shopware: true,
      },
      cmsIntegarion: {
        storyblok: true,
        prepr: true,
      },
    },
  },
  attrs: {
    community: {
      header: false,
      name: "Community",
      isGood: "is good",
      openSource: "open source",
      githubStars: "github stars",
    },
    eCommerce: {
      header: "eCommerce Backend Integration",
      name: "eCommerce",
      magento: "Magento",
      shopify: "Shopify",
      commercetools: "Commercetools",
      shopware: "Shopware",
    },
    cmsIntegarion: {
      header: "CMS Integration",
      name: "CMS",
      storyblok: "Storyblok",
      prepr: "Prepr",
    },
  },
});

const selectedData = ref({ data: {} });
const selectedAttrsCopy = JSON.parse(JSON.stringify(tableData.value.attrs));
const selectedAttrs = ref(selectedAttrsCopy);

const removeSelection = (id: string) => {
  delete selectedData.value.data[id];
};

const addSelection = (id: string) => {
  selectedData.value.data[id] = tableData.value.data[id];
};

const toggleSelectedAttrs = (attrGroupKey: string, attrKey: string) => {
  // console.log(attrGroupKey, attrKey);
  if (
    selectedAttrs.value[attrGroupKey] &&
    selectedAttrs.value[attrGroupKey][attrKey]
  ) {
    delete selectedAttrs.value[attrGroupKey][attrKey];
  } else {
    selectedAttrs.value[attrGroupKey][attrKey] =
      tableData.value.attrs[attrGroupKey][attrKey];
  }
};

// onMounted(() => {
//   console.log("selectedAttrs.value", selectedAttrs.value);
//   console.log("selectedAttrsCopy", selectedAttrsCopy);
// });
</script>

<style lang="scss" scoped>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
  padding: 1rem;

  transition: 2s;
}

.selected {
  background: pink;
}
</style>