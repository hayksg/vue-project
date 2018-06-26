<template>
  <v-container>
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <h1 class="text--secondary mb-3">Create new Ad</h1>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field 
            name="title" 
            label="Ad title " 
            type="text" 
            v-model="title" 
            :rules="[v => !!v || 'Title is required']" 
            required
          ></v-text-field>
          <v-text-field 
            name="description" 
            label="Ad Description " 
            type="text" 
            v-model="description" 
            :rules="[v => !!v || 'Description is required']" 
            required
            multi-line
          ></v-text-field>
          <v-text-field 
            name="price" 
            label="Price " 
            type="text" 
            v-model="price"
            prefix="$"
            :rules="priceRules" 
            required
          ></v-text-field>
        </v-form>
        <v-layout row class="mb-3">
          <v-flex xs12>
            <div class="add-image mb-3">Ad image *</div>
            <img 
              :src="imageSrc" 
              alt="" 
              height="96"
              v-if="imageSrc"
            >
          </v-flex>
        </v-layout>
        <v-layout row class="mb-3">
          <v-flex xs12>
            <v-btn
              class="white--text ml-0"
              color="blue darken-2"
              @click="triggerUpload"
            >
              Upload Image
              <v-icon right dark>cloud_upload</v-icon>
            </v-btn>
            <input 
              type="file"
              ref="fileInput"
              style="display: none;"
              accept="image/*"
              @change="onFileChange"
            >
          </v-flex>
        </v-layout>
        <v-layout row class="mb-3">
          <v-flex xs12>
            <v-switch
              label="Add to slider?"
              v-model="promo"
              color="primary"
            ></v-switch>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs12>
            <v-spacer></v-spacer>
            <v-btn 
              color="success" 
              @click="createAd" 
              class="ml-0" 
              :loading="loading" 
              :disabled="!valid || !image || loading"
            >
              Create ad
            </v-btn>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      title: '',
      description: '',
      price: '',
      promo: false,
      valid: false,
      image: null,
      imageSrc: '',
      priceRules: [
        v => !!v || 'Price is required',
        v => /^[0-9, .]+$/.test(v) || 'Field must be a price'
      ]
    }
  },
  computed: {
    loading () {
      return this.$store.getters.loading
    }
  },
  methods: {
    createAd () {
      if (this.$refs.form.validate() && this.image) {
        const ad = {
          title: this.title,
          description: this.description,
          price: this.price,
          promo: this.promo,
          image: this.image
        }

        this.$store.dispatch('createAd', ad)
            .then(() => {
              this.$router.push('/list')
            })
            .catch(() => {})
      }
    },
    triggerUpload () {
      this.$refs.fileInput.click()
    },
    onFileChange (event) {
      const file = event.target.files[0]

      const reader = new FileReader()
      reader.onload = e => {
        this.imageSrc = reader.result
      }
      reader.readAsDataURL(file)
      this.image = file
    }
  }
}
</script>
<style scoped>
  .add-image { 
    color: rgba(0,0,0,.54);
    font-size: 16px;
  }
</style>