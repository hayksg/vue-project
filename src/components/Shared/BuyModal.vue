<template>
  <v-dialog width="400px" v-model="modal">
    <v-btn class="primary" flat slot="activator">Buy</v-btn>

    <v-card>
      <v-container>
        <v-layout row>
          <v-flex xs12>
            <v-card-title>
              <h1 class="text--primary">Do you wan't to buy it?</h1>
            </v-card-title>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>
        <v-layout row>
          <v-flex xs12>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field
                  name="name"
                  label="Your name"
                  type="text"
                  v-model="name"
                ></v-text-field>
                <v-text-field
                  name="phone"
                  label="Your phone"
                  type="phone"
                  v-model="phone"
                  :rules="phoneRules"
                ></v-text-field>
              </v-form>
            </v-card-text>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>
        <v-layout row>
          <v-flex xs12>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                flat
                outline
                @click="onCancel"
                :disabled="localLoading"
              >
                Close
              </v-btn>
              <v-btn
                class="success"
                flat
                @click="onSave"
                :disabled="localLoading"
                :loading="localLoading"
              >
                Buy it
              </v-btn>
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
        name: '',
        phone: '',
        valid: false,
        localLoading: false,
        phoneRules: [
          v => !!v || 'Phone is required',
          v => /^[0-9() -]+$/.test(v) || 'Please enter a correct phone number'
        ]
      }
    },
    methods: {
      onCancel () {
        this.name = ''
        this.phone = ''
        this.modal = false
      },
      onSave () {
        if (this.$refs.form.validate()) {
          this.localLoading = true
          this.$store.dispatch('createOrder', {
            name: this.name,
            phone: this.phone,
            adId: this.ad.id,
            ownerId: this.ad.ownerId
          })
          .finally(() => {
            this.name = ''
            this.phone = ''
            this.localLoading = false
            this.modal = false
          })
        }
      }
    }
  }
</script>
