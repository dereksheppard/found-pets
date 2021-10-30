<template>
  <div id="app">
    <FoundPets/>
  </div>
</template>

<script>
import HelloWorld from './components/FoundPets.vue'

export default {
  name: 'App',
  components: {
    FoundPets
  }
}
</script>

<style>

[v-cloak] {
  display: none;
}

#app a, #app button {
  cursor: url(https://kingcounty.gov/~/media/depts/regional-animal-services/images/All-Images/paw.ashx), auto;
}

.fadeIn {
  -webkit-animation: fadeIn 1.5s ease;
  animation: fadeIn 1.5s ease;
}
.modal-backdrop {
  opacity: 0.9 !important;
}
.card-body .btn-pets {
  margin-left: 0;
}
.filter-button-row li {
  margin-bottom: .5rem;
}
.filter-button-row button {
  font-size: 12px;
  padding: .25rem;
}

#refine-results label {
  margin-top: 1rem;
}

.pet-image {
  height: 275px;
  object-fit: cover;
  object-position: center;
}
.paws {
  font-size:3rem;
}
.filter-icon, .close-icon {
  font-size: 1rem;
  vertical-align: text-bottom;
}

.color-icon {
  height:34px;
  display: inline-block
}
.results {
  margin-bottom: 1rem;
}
#modal-image-container {
  padding-left: 1rem;
  padding-right: 0;
}

@media only screen and (max-width: 767px) {
  #modal-image-container {
    padding-left: 0;
  }
  #filter-buttons {
    display: none;
  }
}

.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  transform: rotate(45deg)
}

.paws {
  margin-bottom: 2rem;
}

.top-right {
  margin-left: 3rem;
  -webkit-animation: fadeout 2s ease infinite;
  animation: fadeout 2s ease infinite;
  opacity: 0;
  animation-delay: 1.5s;
}

.top-left {
  margin-right: 3rem;
  -webkit-animation: fadeout 2s ease infinite;
  animation: fadeout 2s ease infinite;
  opacity: 0;
  animation-delay: 1s;
}

.bottom-right {
  margin-left: 3rem;
  -webkit-animation: fadeout 2s ease infinite;
  animation: fadeout 2s ease infinite;
  opacity: 0;
  animation-delay: .5s;
}

.bottom-left {
  margin-right: 3rem;
  -webkit-animation: fadeout 2s ease infinite;
  animation: fadeout 2s ease infinite;
  opacity: 0;
  animation-delay: 0s;
}

@-webkit-keyframes fadeout {
  0% {
    opacity: 1;
  }
  25% {
    opacity: 0.75;
  }
  50% {
    opacity: 0.25;
  }
  75% {
    opacity: 0.01;
  }
  100% {
    opacity: 0;
  }
}

@keyframes fadeout {
  0% {
    opacity: 1;
  }
  25% {
    opacity: 0.75;
  }
  50% {
    opacity: 0.25;
  }
  75% {
    opacity: 0.01;
  }
  100% {
    opacity: 0;
  }
}

@-webkit-keyframes fadeIn {
  0% {
    opacity: 0;
  }
  25% {
    opacity: 0.5;
  }
  50% {
    opacity: 0.75;
  }
  75% {
    opacity: 0.9;
  }
  100% {
    opacity: 1;
  }
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  25% {
    opacity: 0.5;
  }
  50% {
    opacity: 0.75;
  }
  75% {
    opacity: 0.9;
  }
  100% {
    opacity: 1;
  }
}

</style>

