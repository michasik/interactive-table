<template>
  <table>
    <!-- MAIN HEADER ROW -->
    <tr>
      <th>🔎</th>
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
        v-if="tableData.attrs[groupKey].header"
        :key="index + tableData.attrs[groupKey].header"
      >
        <td colspan="100%">{{ tableData.attrs[groupKey].header }}</td>
      </tr>

      <!-- VERTICAL HEADERS -->
      <template
        v-for="attr in Object.keys(tableData.attrs[groupKey])"
        :key="attr.toString()"
      >
        <tr>
          <th v-if="attr !== 'header'">
            {{ tableData.attrs[groupKey][attr] }}
          </th>

          <!-- ACTUAL DATA -->
          <template v-for="data in selectedData.data">
            <td v-if="attr !== 'header'" class="selected">
              {{ data[groupKey][attr] }}
            </td>
          </template>
          <template v-for="data in tableData.data">
            <td
              v-if="
                attr !== 'header' &&
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
</template>

<script setup lang="ts">
import { ref } from "vue";
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
      isGood: "is good",
      openSource: "open source",
      githubStars: "github stars",
    },
    eCommerce: {
      header: "eCommerce Backend Integration",
      magento: "Magento",
      shopify: "Shopify",
      commercetools: "Commercetools",
      shopware: "Shopware",
    },
    cmsIntegarion: {
      header: "CMS Integration",
      storyblok: "Storyblok",
      prepr: "Prepr",
    },
  },
});

const selectedData = ref({
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
});

const removeSelection = (id: string) => {
  delete selectedData.value.data[id];
};

const addSelection = (id: string) => {
  selectedData.value.data[id] = tableData.value.data[id];
};
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