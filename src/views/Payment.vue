<template>
<v-container>
      <v-form justify="center" align="center" class="mb-10">
         <v-row justify="center" class="mt-10">
            <v-col md="4" sm="9" xsm="9">
               <h2 class="text-center">Unesite podatke o placanju</h2>
               <v-text-field
                  name="Broj kartice"
                  label="Broj kartice"
                  type="number"
                  v-model="firstName"
               ></v-text-field>
               <v-text-field
                  name="Ime i prezime"
                  type="text"
                  label="Ime i prezime"
                  v-model="lastName"
               ></v-text-field>
               <div>
                  <v-menu
                     ref="menu"
                     v-model="menu"
                     :close-on-content-click="false"
                     transition="scale-transition"
                     offset-y
                     min-width="auto"
                  >
                     <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                           v-model="date"
                           label="Datum isteka kartice"
                           readonly
                           v-bind="attrs"
                           v-on="on"
                        ></v-text-field>
                     </template>
                     <v-date-picker
                        v-model="date"
                        :active-picker.sync="activePicker"
                        :max="
                           new Date(
                              Date.now() -
                                 new Date().getTimezoneOffset() * 60000
                           )
                              .toISOString()
                              .substr(0, 10)
                        "
                        min="1950-01-01"
                        @change="save"
                     ></v-date-picker>
                  </v-menu>
               </div>
               <v-text-field
                  name="CVV"
                  type="CVV"
                  label="CVV"
                  required
                  v-model="CVV"
               ></v-text-field>
               
               <v-btn color="primary" @click.prevent="signUpUser"
                  >Sign Up</v-btn>
            </v-col>
         </v-row>
      </v-form>
   </v-container>

</template>

<script>

export default {
	name: "Payment",
};

</script>

<style></style>
