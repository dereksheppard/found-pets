<template>
  <div role="region" class="container">
    <!--LOADING ICON START -->

    <section v-if="loading" class="loading-container">
      <span class="sr-only">Loading...</span>
      <span class="paws top-right material-icons">pets</span>
      <span class="paws top-left material-icons">pets</span>
      <span class="paws bottom-right material-icons">pets</span>
      <span class="paws bottom-left material-icons">pets</span>
    </section>

    <!--LOADING ICON END -->

    <div class="fadeIn" v-cloak>
      <section id="errorMessage" v-if="errored">
        <div class="alert alert-danger" role="alert">
          <p class="h4">
            <span class="fa fa-exclamation-circle"></span> We're sorry, we're
            not able to retrieve this information at the moment, please try back
            later.
          </p>
        </div>
      </section>

      <!-- CARD LIST START -->

      <!--CARD ROW CONTAINER START -->
      <div v-if="!loading && !errored">
        <!-- FILTER START -->

        <div class="row border" style="padding-bottom:1rem;">
          <div class="col-md-4">
            <label for="postSearch">Search by Name</label>
            <input
              type="text"
              id="postSearch"
              class="form-control"
              v-model="search"
            />
          </div>

          <div class="col-md-4">
            <label for="typeSelect">Animal Type</label>
            <select id="typeSelect" class="form-control" v-model="selectedType">
              <option value="">All</option>
              <option value="cat">Cat</option>
              <option value="dog">Dog</option>
              <option value="bird">Bird</option>
              <option value="livestock">Livestock</option>
              <option value="other">
                Other (rodents, rabbits, reptiles, etc.)
              </option>
            </select>
          </div>
          <div class="col-md-4">
            <label for="idSearch">Search by ID </label>
            <input
              type="text"
              id="idSearch"
              class="form-control"
              v-model="searchId"
            />
          </div>

        </div>

        

        <!-- FILTER END -->

        <div
          style="margin-top: 2rem;"
          class="row"
          v-bind:key="i"
          v-for="i in Math.ceil(computed_items.length / 2)"
        >
          <div
            v-bind:key="i"
            v-for="(items, i) in computed_items.slice((i - 1) * 2, i * 2)"
            v-bind:class="{
              'col-md-6  mb-3': computed_items.length > 1,
              'col-sm-12': computed_items.length <= 1,
            }"
          >
            <div class="card h-100 shadow">
              <img v-if="items.image"
                class="card-img-top pet-image"
                v-bind:src="items.image.url"
                v-bind:alt="items.animal_id + ' the ' + items.animal_type"
                loading="lazy"
              />

              <div class="card-body pb-1">
                <h3 v-bind:id="items.animal_id" class="card-title float-left">
                  {{ items.animal_name || items.animal_id }}
                </h3>
                

              </div>
              <div class="card-body pt-0">
                <ul class="list-unstyled">
                  <li>{{ items.animal_type }}</li>
                  <li>
                    {{ items.animal_gender | removeSpayed }}
                    {{ items.animal_breed | lowercase }}
                  </li>
                  <li><span v-html="items.memo"></span></li>
                </ul>
                
              </div>
            </div>

      

          </div>

          <!-- NO RESULTS MESSAGE START -->

          <!-- NO RESULTS MESSAGE END -->
        </div>
        <!--STUFF GOES IN HERE-->

        <section
          class="row"
          id="noResults"
          v-if="!loading && !computed_items.length && !errored"
        >
          <div class="col alert" role="alert">
            <p class="h1 text-danger">Hmm...</p>
            <p>
              We tried to sniff out animals matching your search, but didn't
              find any. A different search or filter combination might find more
              results!
            </p>

            <div class="text-center">
              <img
                class="img-fluid"
                src="https://kingcounty.gov/~/media/depts/regional-animal-services/images/banners/dog-search-results.ashx"
                alt=""
              />
            </div>
          </div>
        </section>

        <!-- CARD ROW CONTAINER END -->
      </div>
    </div>
    <!--CLOAK -->
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'PetsLookup',
  components: {},

  data () {
    return {
      info: [],
      loading: true,
      errored: false,
      search: '',
      searchId: '',
      selectedType: '',
      selectedId: '',
      selectedTemp: '',
      selectedAge: [],
      selectedLocation: '',
      selectedSex: ''
    }
  },

  updated () {},
  mounted () {
    axios
      .get(
        'https://data.kingcounty.gov/resource/yaai-7frk.json?$$app_token=KBohEkNYDLIPOD9wwrlUVYvEn&record_type=FOUND&$order=date%20DESC'
      )

      .then((response) => {
        this.info = response.data

        // console.log(response);
      })
      .catch((error) => {
        console.log(error)
        this.errored = true
      })

      .finally(() => (this.loading = false))
  },

  computed: {
    itemsCount () {
      return this.computed_items.length
    },
    lessThan () {
      return this.info.length
    },

    computed_items: function () {
      const filterSearch = this.search
      const filterId = this.searchId
      const filterType = this.selectedType
     
      

      return this.info.filter(function (item) {
        // SEARCH FUNCTION

        let filtered = true

        if (filtered) {
          if (filterSearch.length > 1) {
            if (item.animal_name !== undefined) {
              filtered = item.animal_name
                .toLowerCase()
                .includes(filterSearch.toLowerCase())
            }
            if (item.animal_name === undefined) {
              return false
            }
          }
        }
        if (filtered) {
          if (filterId.length > 1) {
            if (item.animal_id !== undefined) {
              filtered = item.animal_id
                .toUpperCase()
                .includes(filterId.toUpperCase())
            }
            if (item.animal_id === undefined) {
              return false
            }
          }
        }
    

        if (filtered) {
          if (filterType && filterType.length > 0) {
            if (item.animal_type !== undefined) {
              if (filterType === '') {
                filtered = item.animal_type === filterType
              } else {
                if (item.animal_type.toString().toLowerCase().includes('cat')) {
                  var cat = 'cat'
                  filtered = cat === filterType
                }
                if (item.animal_type.toString().toLowerCase().includes('dog')) {
                  var dog = 'dog'
                  filtered = dog === filterType
                }
                if (
                  item.animal_type.toString().toLowerCase().includes('bird')
                ) {
                  var bird = 'bird'
                  filtered = bird === filterType
                }
                if (
                  item.animal_type
                    .toString()
                    .toLowerCase()
                    .includes('livestock')
                ) {
                  var livestock = 'livestock'
                  filtered = livestock === filterType
                }
                if (
                  !item.animal_type.toString().toLowerCase().includes(cat) &&
                  !item.animal_type.toString().toLowerCase().includes(dog) &&
                  !item.animal_type.toString().toLowerCase().includes(bird) &&
                  !item.animal_type.toString().toLowerCase().includes(livestock)
                ) {
                  var other = 'other'
                  filtered = other === filterType
                }
              }
            }
            if (item.animal_gender === undefined) {
              return false
            }
          }
        }





        return filtered
      })
    },

    info_type: function () {
      return [
        ...new Set(
          this.info
            .map((i) => i.animal_type)
            .slice()
            .sort()
        )
      ]
    },

    info_location: function () {
      return [
        ...new Set(
          this.info
            .map((i) => i.current_location)
            .slice()
            .sort()
        )
      ]
    }
  },

  methods: {
    resetForm: function (e) {
      e.preventDefault()
      this.search = ''
      this.searchId = ''
      this.selectedType = ''
      this.selectedAge = []
      this.selectedTemp = ''
      this.selectedLocation = ''
      this.selectedSex = ''
      this.ageClear()
    },
    ageClear: function () {
      var ageSelect = document.getElementById('ageSelect').options
      for (var i = 0; i < ageSelect.length; i++) {
        ageSelect[i].selected = false
      }
    }
  },

  filters: {
    titlecase: function (value) {
      if (!value) return ''
      value = value.toString().toLowerCase()

      return value.charAt(0).toUpperCase() + value.substring(1)
    },
    lowercase: function (value) {
      if (!value) return ''
      value = value.toString().toLowerCase()

      return value
    },
    dateformat: function (value) {
      var date = new Date(value)
      var formatdate = new Intl.DateTimeFormat('en-US').format(date)
      return formatdate
    },
    removeSpayed: function (value) {
      var removed = value.charAt(0).toUpperCase() + value.substring(1)

      if (value.toString().toLowerCase().includes('spayed')) {
        return removed.replace('Spayed', '')
      }
      if (value.toString().toLowerCase().includes('neutered')) {
        return removed.replace('Neutered', '')
      } else {
        return value
      }
    }
  }
}
</script>


