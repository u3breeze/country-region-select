<script setup lang="ts">
import { computed } from 'vue';
import regions from '../../../../countryRegionData';

// Defining the props with types
const props = withDefaults(defineProps<{
  country?: string;
  countryName?: boolean;
  whiteList?: string[];
  blackList?: string[];
  className?: string;
  shortCodeDropdown?: boolean;
  autocomplete?: boolean;
  topCountry?: string;
  placeholder?: string;
  disablePlaceholder?: boolean;
  removePlaceholder?: boolean;
}>(), {
  topCountry: '',
  placeholder: 'Select Country',
  disablePlaceholder: false,
  removePlaceholder: false,
});


// Reactive data
// const ran = ref(false);

const emit = defineEmits(["countrySelected"]);

// Computed properties
const countries = computed(() => {
  let countryList = regions.filter((region) => {
    if (props.countryName) {
      return region.countryName !== firstCountry.value;
    } else {
      return region.countryShortCode !== firstCountry.value;
    }
  });

  if (props.whiteList) {
    countryList = countryList.filter((country) => {
      return props.whiteList?.includes(country.countryShortCode);
    });
  }

  if (props.blackList) {
    countryList = countryList.filter((country) => {
      return !props.blackList?.includes(country.countryShortCode);
    });
  }

  if (props.removePlaceholder) {
    let c = firstCountry.value || countryList[0][valueType.value];
    onChange(c);
  }

  return countryList;
});

const firstCountry = computed(() => {
  if (props.countryName) {
    if (props.topCountry && props.topCountry.length === 2) {
      const regionObj = regions.find((region) => region.countryShortCode === props.topCountry);
      return regionObj ? regionObj.countryName : '';
    } else {
      return props.topCountry || '';
    }
  }
  return props.topCountry || '';
});

const value = computed(() => {
  return props.country || '';
});

const valueType = computed(() => {
  return props.countryName ? 'countryName' : 'countryShortCode';
});

const autocompleteAttr = computed(() => {
  const autocompleteType = (showsFullCountryName: boolean) => showsFullCountryName ? "country-name" : "country";
  return props.autocomplete ? autocompleteType(props.countryName || false) : "off";
});

// Methods
const onChange = (country?: string) => {
  emit('countrySelected', country);
};

const topCountryName = computed(() => {
  const regionObj = regions.find((region) => {
    if (props.countryName) {
      return region.countryName === firstCountry.value;
    } else {
      return region.countryShortCode === firstCountry.value;
    }
  });

  if (!regionObj) return '';

  return props.shortCodeDropdown ? regionObj.countryShortCode : regionObj.countryName;
});
</script>

<template>
  <select @change="onChange($event.target.value)" :class="className" :autocomplete="autocompleteAttr">
    <option value="" v-if="!disablePlaceholder && !removePlaceholder">{{ placeholder }}</option>
    <option value="" v-if="disablePlaceholder && !removePlaceholder" disabled selected>{{ placeholder }}</option>
    <option v-if="topCountry" :value="firstCountry" :selected="country === firstCountry">{{ topCountryName }}</option>
    <option v-for="(region, index) in countries" :value="region[valueType]" :selected="country === region[valueType]" :key="index">{{ shortCodeDropdown ? region.countryShortCode : region.countryName }}</option>
  </select>
</template>
