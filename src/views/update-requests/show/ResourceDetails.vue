<template>
  <ck-loader v-if="loading"/>

  <div v-else>
    <gov-body>
      For resource
      <gov-link
        :to="{
          name: 'resources-show',
          params: { resource: original.id }
        }"
        v-text="original.name"
      />.
    </gov-body>

    <gov-table>
      <template slot="body">
        <gov-table-row>
          <gov-table-header scope="column" />
          <gov-table-header scope="column">From</gov-table-header>
          <gov-table-header scope="column">To</gov-table-header>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('organisation_id')">
          <gov-table-header top scope="row">Organisation</gov-table-header>
          <gov-table-cell>
            <gov-link
              :to="{
                name: 'organisations-show',
                params: { organisation: original.organisation_id }
              }"
            >
              {{ original.organisation.name }}
            </gov-link>
          </gov-table-cell>
          <gov-table-cell>
            <gov-link
              :to="{
                name: 'organisations-show',
                params: { organisation: resource.organisation_id }
              }"
            >
              {{ resource.organisation.name }}
            </gov-link>
          </gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('name')">
          <gov-table-header top scope="row">Name</gov-table-header>
          <gov-table-cell break>{{ original.name }}</gov-table-cell>
          <gov-table-cell break>{{ resource.name }}</gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('slug')">
          <gov-table-header top scope="row">Slug</gov-table-header>
          <gov-table-cell break>{{ original.slug }}</gov-table-cell>
          <gov-table-cell break>{{ resource.slug }}</gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('description')">
          <gov-table-header top scope="row">Description</gov-table-header>
          <gov-table-cell v-html="original.description"/>
          <gov-table-cell v-html="resource.description"/>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('url')">
          <gov-table-header top scope="row">URL</gov-table-header>
          <gov-table-cell break>{{ original.url }}</gov-table-cell>
          <gov-table-cell break>{{ resource.url }}</gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('license')">
          <gov-table-header top scope="row">License</gov-table-header>
          <gov-table-cell break>{{ original.license || '-' }}</gov-table-cell>
          <gov-table-cell break>{{ resource.license || '-' }}</gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('author')">
          <gov-table-header top scope="row">Author</gov-table-header>
          <gov-table-cell break>{{ original.author || '-' }}</gov-table-cell>
          <gov-table-cell break>{{ resource.author || '-' }}</gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('category_taxonomies')">
          <gov-table-header top scope="row">Topic taxonomies</gov-table-header>
          <gov-table-cell>
            <gov-list bullet v-if="original.category_taxonomies.length > 0">
              <li
                v-for="(taxonomy, index) in original.category_taxonomies"
                :key="`category_taxonomies.${index}`"
              >
                {{ taxonomyName(findTaxonomy(taxonomy.id)) }}
              </li>
            </gov-list>
            <template v-else>None</template>
          </gov-table-cell>
          <gov-table-cell>
            <gov-list bullet v-if="resource.category_taxonomies.length > 0">
              <li
                v-for="(taxonomy, index) in resource.category_taxonomies"
                :key="`category_taxonomies.${index}`"
              >
                {{ taxonomyName(findTaxonomy(taxonomy)) }}
              </li>
            </gov-list>
            <template v-else>None</template>
          </gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('published_at')">
          <gov-table-header top scope="row">Published date</gov-table-header>
          <gov-table-cell break>
            {{ original.published_at ? formatDate(original.published_at): '-' }}
          </gov-table-cell>
          <gov-table-cell break>
            {{ resource.published_at ? formatDate(resource.published_at) : '-' }}
          </gov-table-cell>
        </gov-table-row>

        <gov-table-row v-if="resource.hasOwnProperty('last_modified_at')">
          <gov-table-header top scope="row">Last modified date</gov-table-header>
          <gov-table-cell break>
            {{ original.last_modified_at ? formatDate(original.last_modified_at) : '-' }}
          </gov-table-cell>
          <gov-table-cell break>
            {{ resource.last_modified_at ? formatDate(resource.last_modified_at) : '-' }}
          </gov-table-cell>
        </gov-table-row>
      </template>
    </gov-table>
  </div>
</template>

<script>
import http from "@/http";

export default {
  name: "ResourceDetails",

  props: {
    updateRequestId: {
      required: true,
      type: String
    },

    requestedAt: {
      required: true,
      type: String
    },

    resource: {
      required: true,
      type: Object
    }
  },

  data() {
    return {
      loading: false,
      original: null,
      taxonomies: [],
      flattenedTaxonomies: []
    };
  },

  methods: {
    taxonomyName(taxonomy) {
      let name = taxonomy.name;

      if (taxonomy.parent_id !== null) {
        const parent = this.flattenedTaxonomies.find(flattenedTaxonomy => {
          return flattenedTaxonomy.id === taxonomy.parent_id;
        });
        name = `${this.taxonomyName(parent)} / ${name}`;
      }

      return name;
    },

    async fetchAll() {
      this.loading = true;

      await this.fetchOriginal();
      await this.fetchTaxonomies();

      this.loading = false;
    },

    async fetchOriginal() {
      const {
        data: { data: original }
      } = await http.get(`/resources/${this.resource.id}`, {
        params: { include: "organisation" }
      });

      this.original = original;
    },

    async fetchTaxonomies() {
      const {
        data: { data: taxonomies }
      } = await http.get("/taxonomies/categories");
      this.taxonomies = taxonomies;
      this.setFlattenedTaxonomies();
    },

    setFlattenedTaxonomies(taxonomies = null) {
      if (taxonomies === null) {
        this.flattenedTaxonomies = [];
        taxonomies = this.taxonomies;
      }

      taxonomies.forEach(taxonomy => {
        this.flattenedTaxonomies.push(taxonomy);

        if (taxonomy.children.length > 0) {
          this.setFlattenedTaxonomies(taxonomy.children);
        }
      });
    },

    findTaxonomy(id) {
      return this.flattenedTaxonomies.find(taxonomy => taxonomy.id === id);
    }
  },

  created() {
    this.fetchAll();
  }
};
</script>
