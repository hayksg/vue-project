<template>
  <v-dialog
    v-model="modal"
    width="500px"
  >
    <v-btn outline color="primary" slot="activator">Edit</v-btn>
    <v-card>
      <v-container>
        <v-layout row>
          <v-flex xs12>
            <v-card-title>
              <h1 class="text--primary">Edit ad</h1>  
            </v-card-title>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>
        <v-layout row>
          <v-flex xs12>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field 
                  name="title"
                  label="Title"
                  type="text"
                  v-model="editedTitle"
                ></v-text-field>
                <v-text-field 
                  name="description"
                  label="Description"
                  type="text"
                  multi-line
                  v-model="editedDescription"
                ></v-text-field>
                <v-text-field
                  name="price"
                  label="Price"
                  type="text"
                  prefix="$"
                  :rules="priceRules"
                  v-model="editedPrice"
                ></v-text-field>
              </v-form>
            </v-card-text>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs12>
            <v-card-actions>
              <v-btn class="info" @click="onCancel">Cancel</v-btn>
              <v-spacer></v-spacer>
              <v-btn class="success" @click="onSave">Save</v-btn>
            </v-card-actions>
          </v-flex>
        </v-layout>
      </v-container>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['ad'],
  data () {
    return {
      modal: false,
      editedTitle: this.ad.title,
      editedDescription: this.ad.description,
      editedPrice: this.ad.price,
      valid: false,
      priceRules: [
        v => !!v || 'Price is required',
        v => /^[0-9, .]+$/.test(v) || 'Field must be a price'
      ]
    }
  },
  methods: {
    onCancel () {
      this.editedDescription = this.ad.description
      this.editedTitle = this.ad.title
      this.editedPrice = this.ad.price
      this.modal = false
    },
    onSave () {
      if (this.$refs.form.validate()) {
        this.$store.dispatch('updateAd', {
          title: this.editedTitle,
          description: this.editedDescription,
          price: this.editedPrice,
          id: this.ad.id
        })

        this.modal = false
      }
    }
  }
}
</script>