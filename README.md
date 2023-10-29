## Country and region select components
Implemented with Vue3, Tailwind CSS, TypeScript.

## How to use
You need to use the three files, ./src/components/ui/inputs/country-region-select/country-select.vue and region-select.vue, and data files: ./src/countryRegionData.ts
Then in your own vue, for example:

    <script setup lang="ts">
    import { ref } from "vue";
    import CountrySelect from "@src/components/ui/inputs/country-region-select/country-select.vue";
    import RegionSelect from "@src/components/ui/inputs/country-region-select/region-select.vue";
    
    const countrySelected = ref("");
    const regionSelected = ref("");
    
    </script>
    
    <template>
    
      <div class="m-10">
    
        <div class="flex flex-col w-2/3">
          <country-select placeholder="All Country" :country="countrySelected" @countrySelected="country => countrySelected = country"
                          class-name="h-8 mb-4 border-2  px-4 rounded-sm bg-transparent text-sm text-black opacity-60 dark:text-white dark:opacity-70 font-normal leading-4 tracking-[0.16px]"
          />
          <region-select  placeholder="All Region"  :country="countrySelected" :region="regionSelected" @regionSelected="region => regionSelected = region"
                          class-name="h-8 border-2 px-4 rounded-sm bg-transparent text-sm text-black opacity-60 dark:text-white dark:opacity-70 font-normal leading-4 tracking-[0.16px]"
          />
        </div>
    
      </div>
    
    </template>

![shot.jpg](shot.jpg)
