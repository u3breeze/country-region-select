<script setup lang="ts">
import {computed, onMounted, ref, Ref, watch} from 'vue';
import regions from '../../../../countryRegionData';

const props = defineProps({
  country: String,
  region: String,
  defaultRegion: String,
  countryName: Boolean,
  whiteList: Array,
  blackList: Array,
  regionName: Boolean,
  className: String,
  shortCodeDropdown: Boolean,
  autocomplete: Boolean,
  placeholder: {
    type: String,
    default: 'Select Region'
  },
  disablePlaceholder: {
    type: Boolean,
    default: false
  },
  removePlaceholder: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['regionSelected']);


type Region = {
  name: string;
  shortCode?: string;
};

// Reactive state
const shownRegions: Ref<Region[]> = ref([]);
const ran = ref(false);



// Methods
const getRegionWithCountry = (country?: string) => {
  country = country || props.country;
  // ... (rest of your original method logic)

  shownRegions.value = regions.find((elem) => {
    if (props.countryName) {
      return elem.countryName === country;
    } else {
      return elem.countryShortCode === country;
    }
  })?.regions || []

  if (props.disablePlaceholder && ran.value) {
    const selectedRegion = shownRegions.value[0][valueType.value];
    if (selectedRegion) {
      onChange(selectedRegion);
    }
  }
  if (props.removePlaceholder) {
    const selectedRegion = shownRegions.value[0][valueType.value];
    if (selectedRegion) {
      onChange(selectedRegion);
    }
  }
  ran.value = true;
};

const onChange = (region: string) => {
  emit('regionSelected', region);
};

// Computed properties
const valueType = computed(() => {
  return props.regionName ? 'name' : 'shortCode';
});

const autocompleteAttr = computed(() => {
  return props.autocomplete ? "address-level1" : "off";
});

// Watchers and lifecycle hooks
watch(() => props.country, (newVal, oldVal) => {
  if (oldVal !== '') {
    onChange('');
  }
  if (props.country) {
    getRegionWithCountry();
  } else {
    shownRegions.value = [];
  }
});

onMounted(() => {
  if (props.country) {
    getRegionWithCountry();
  } else {
    let findRegion = '';
    if (props.countryName) {
      findRegion = props.defaultRegion ? props.defaultRegion : 'United States';
    } else {
      findRegion = props.defaultRegion ? props.defaultRegion : 'US';
    }
    getRegionWithCountry(findRegion);
  }
});

</script>

<template>
  <select @change="onChange($event.target.value)"
          :class="props.className"
          :autocomplete="autocompleteAttr"
  >
    <option v-if="!props.disablePlaceholder && !props.removePlaceholder" value="">{{ props.placeholder }}</option>
    <option v-if="props.disablePlaceholder && !props.removePlaceholder" value="" disabled selected>{{ props.placeholder }}</option>
    <option v-for="(place, index) in shownRegions" :key="index" :value="place[valueType] !== '' ? place[valueType] : place.name.substring(0,3)" :selected="props.region === place[valueType]">{{props.shortCodeDropdown ? place.shortCode : place.name}}</option>
  </select>
</template>
