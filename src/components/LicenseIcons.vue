<template>
  <a :href="getLicenseURL(image)"
      @click.stop="() => false"
     class="photo-license-icons"
     target="_blank"
     rel="noopener noreferrer">
    <img class="photo-license-icon" alt="CC" src="@/assets/cc_icon.svg" />
    <template v-for="(license, index) in onGetLicenseIcon(image.license)">
      <img
          v-if="license" class="photo-license-icon"
          :alt="`${license.toUpperCase()}`"
          :src="require(`@/assets/cc-${license.toLowerCase()}_icon.svg`)"
          :key="index" />
    </template>
  </a>
</template>

<script>
const LicenseIcons = {
  name: 'license-icons',
  components: {},
  props: {
    image: '',
    shouldWrapInLink: false,
  },
  methods: {
    onGetLicenseIcon(license) {
      let licenses = [];
      if (license) {
        licenses = license.split('-');
      }
      return licenses;
    },
    getLicenseURL(image) {
      if (!image) {
        return '';
      }

      const BASE_URL = 'https://creativecommons.org';
      let url = `${BASE_URL}/licenses/${image.license}/${image.license_version}`;
      let license = '';

      if (image.license) {
        license = image.license;
      }

      if (license === 'cc0') {
        this.image.license_version = '1.0';
        url = `${BASE_URL}/publicdomain/zero/1.0/`;
      }
      else if (image.license === 'pdm') {
        url = `${BASE_URL}/publicdomain/mark/1.0/`;
      }

      return url;
    },
  },
};

export default LicenseIcons;
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  .photo-license-icons {
    height: 1.875rem;
  }

  .photo-license-icon {
    height: inherit;
    margin-right: .3rem;
    vertical-align: middle;
  }
</style>
