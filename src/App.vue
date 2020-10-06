<template>
  <div>
    <Banner />

    <div class="container">
      <ais-instant-search :search-client="searchClient" index-name="Teds">
        <ais-stats>
          <p
            slot-scope="{
              hitsPerPage,
              nbPages,
              nbHits,
              page,
              processingTimeMS,
              query,
            }"
            class="text--italic"
          >
            Page {{ page + 1 }} of {{ nbPages }} with {{ hitsPerPage }} hits per
            page - {{ nbHits }} hits retrieved in {{ processingTimeMS }}ms for
            <q>{{ query }} </q> - Yeahh 👏
          </p>
        </ais-stats>
        <div class="search-panel">
          <div class="search-panel__filters">
            <ais-clear-refinements />
            <h2>FILTERS</h2>
            <ais-refinement-list attribute="tags" :limit="15" searchable />
            <ais-configure :hitsPerPage="8" />
            <p>DURATION</p>
            <p class="text--italic text--small">
              1 is under 10', 4 is above 30'
            </p>
            <ais-range-input attribute="duration_range">
              <div slot-scope="{ currentRefinement, range, refine }">
                <vue-slider
                  :min="range.min"
                  :max="range.max"
                  :lazy="true"
                  :value="toValue(currentRefinement, range)"
                  @change="refine({ min: $event[0], max: $event[1] })"
                />
              </div>
            </ais-range-input>
            <p>POPULARITY</p>
            <p class="text--italic text--small">
              Number of people who vote for this speech
            </p>
            <ais-range-input attribute="popularity_score">
              <div slot-scope="{ currentRefinement, range, refine }">
                <vue-slider
                  :min="range.min"
                  :max="range.max"
                  :lazy="true"
                  :value="toValue(currentRefinement, range)"
                  @change="refine({ min: $event[0], max: $event[1] })"
                />
              </div>
            </ais-range-input>
            <p>LANGUAGES</p>
            <p class="text--italic text--small">
              Subtitles available in more than 100 languages
            </p>
            <ais-menu-select attribute="languages" :limit="200" />
          </div>

          <div class="search-panel__results">
            <ais-search-box
              placeholder="Search for a speech..."
              class="searchbox"
            />
            <ais-infinite-hits :show-previous="true">
              <!-- <ais-configure :hits-per-page.camel="14" /> -->
              <button
                slot="loadPrevious"
                slot-scope="{ page, isFirstPage, refinePrevious }"
                :disabled="isFirstPage"
                @click="refinePrevious"
              >
                Show previous results (page: {{ page + 1 }})
              </button>

              <template slot="item" slot-scope="{ item }" class="mainDom">
                <h1><ais-highlight :hit="item" attribute="name" /></h1>
                <img
                  :src="item.image_url"
                  :hit="item"
                  attribute="image_url"
                  class="mainDom--img"
                />
                <div class="hit-description">
                  <p>{{ item.description }}</p>
                  <p class="speakers">{{ item.speakers }}</p>
                  <p class="views">
                    <i class="fab fa-youtube"></i> {{ item.viewed_count }}
                  </p>
                </div>
              </template>
            </ais-infinite-hits>

            <div class="pagination"><ais-pagination /></div>
          </div>
        </div>
      </ais-instant-search>
    </div>
  </div>
</template>

<script>
import algoliasearch from "algoliasearch/lite";
import "instantsearch.css/themes/algolia-min.css";
import VueSlider from "vue-slider-component";
import "vue-slider-component/theme/default.css";
import Banner from "../components/Banner";

export default {
  components: {
    VueSlider,
    Banner,
  },
  data() {
    return {
      searchClient: algoliasearch(
        process.env.VUE_APP_CLIENT,
        process.env.VUE_APP_API_KEY
      ),
    };
  },
  methods: {
    toValue(value, range) {
      return [
        value.min !== null ? value.min : range.min,
        value.max !== null ? value.max : range.max,
      ];
    },
  },
};
</script>

<style lang="scss">
//GLOBAL STYLES

body,
h1 {
  margin: 0;
  padding: 0;
}

h1,
h2,
h3,
p {
  color: #454141;
}

.text--italic {
  font-style: italic;
}
.text--small {
  font-size: 10px;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  background-color: white;
}

.container {
  margin: 2rem 1rem 2rem 1rem;
  padding: 2rem;
  box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.5);
  border-radius: 30px;
  background-color: rgb(255, 253, 253);
}

// RIGHT PANEL

.ais-Stats p {
  text-align: right;
  font-size: 12px;
  color: #454141;
}

.ais-SearchBox-input {
  border-radius: 30px;
}

.ais-ClearRefinements-button,
.ais-InfiniteHits-loadMore {
  border-radius: 30px;
  padding: 10px 20px 10px 20px;
  background-color: #f90f00;
  cursor: pointer;
  transition: all 0.5s ease-in-out;

  &:hover {
    background-color: #454141;
    color: rgb(248, 247, 247);
    transition: all 0.5s ease-in-out;
  }
}

.ais-InfiniteHits-loadMore {
  transform: translateX(25vw);
  margin: 2rem auto 2rem auto;
  background-color: #f90f00;
}

.ais-InfiniteHits-list {
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(3, 30%);
  margin: 0 auto;
  justify-content: center;

  @media screen and (max-width: 992px) {
    grid-template-columns: repeat(2, 45%);
  }
  @media screen and (max-width: 772px) {
    grid-template-columns: repeat(1, 80%);
  }
}
.mainDom--img {
  width: 50%;
  margin: 0 auto;
  border-radius: 10px;
  justify-self: center;
  margin-bottom: 1rem;
  filter: contrast(120%);
  transition: all 0.3s ease-in-out;
}

.ais-InfiniteHits-item {
  width: 100%;
  display: flex;
  flex-direction: column;
  border: none;
  box-shadow: 5px 5px 15px rgba(249, 15, 0, 0.1);
  border-radius: 30px;
  transition: all 0.2s ease-in-out;
  cursor: pointer;

  h1 {
    text-align: center;
    margin-bottom: 1rem;
    color: #454141;
    font-size: 16px;
  }

  &:hover {
    transition: all 0.3s ease-in-out;
    transform: scale(1.1);

    img {
      filter: contrast(180%);
      transition: all 0.3s ease-in-out;
    }
  }
}

.hit-description {
  p {
    font-size: 12px;
    text-align: center;
  }

  .speakers {
    font-size: 10px;
    text-align: right;
    color: #f90f00;
    font-style: italic;
    font-weight: bold;
  }

  .views {
    margin-top: 2rem;
    text-align: right;
    .fa-youtube {
      color: #f90f00;
    }
  }
}

//PAGINATION

.ais-Pagination-item,
.ais-Pagination-link {
  border: none !important;
  background-color: transparent !important;
  color: #454141;
  font-family: "Poppins", serif !important;
}

.ais-Pagination-item--selected {
  border: #f90f00 !important;
  background-color: #f90f00 !important;
  border-radius: 30px;
  color: white !important;
  font-family: "Poppins", serif !important;
}

//LEFT PANEL

.search-panel {
  display: flex;
}

.search-panel__filters {
  flex: 1;
  margin-right: 1em;
  box-shadow: 5px 5px 15px rgba(249, 15, 0, 0.1);
  border-radius: 30px;
  padding: 1rem;
}

.ais-RefinementList-list {
  margin: 1rem auto 4rem;
}

.vue-slider-ltr {
  margin: 1rem auto 4rem;
}

.ais-RefinementList-searchBox {
  display: none;
}

.ais-MenuSelect-select {
  border-radius: 30px;
}

.search-panel__results {
  flex: 3;
}

.searchbox {
  margin-bottom: 2rem;
}

.pagination {
  margin: 2rem auto;
  text-align: center;
}
</style>
