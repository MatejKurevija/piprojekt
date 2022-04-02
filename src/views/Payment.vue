<template>
<v-container>
      <v-form justify="center" align="center" class="mb-10">
         <v-row justify="center" class="mt-10">
            <v-col md="4" sm="9" xsm="9">
               <h2 class="text-center">Insert payment info</h2>
               <v-text-field
                  name="Credit card number"
                  label="Credit card number"
                  type="number"
                  counter ="16"
               ></v-text-field>
               <v-text-field
                  name="Last name"
                  type="text"
                  label="Last name"
               ></v-text-field>
               <div>
                  <v-menu
                     ref="menu"
                     :close-on-content-click="false"
                     transition="scale-transition"
                     offset-y
                     min-width="auto"
                  >
                     <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                           label="Expiration date"
                           readonly
                           v-bind="attrs"
                           v-on="on"
                        ></v-text-field>
                     </template>
                     <v-date-picker
                        :max="
                           new Date(
                              Date.now() -
                                 new Date().getTimezoneOffset() * 60000
                           )
                              .toISOString()
                              .substr(0, 10)
                        "
                        min="1950-01-01"
                     ></v-date-picker>
                  </v-menu>
               </div>
               <v-text-field
                  name="CVC"
                  type="CVC"
                  label="CVC"
                  required
                  counter="3"
               ></v-text-field>
               <div type="text">Review the renter</div>
               <v-rating
               color="warning"
               background-color="grey"
               hover
               v-model="rating"
         ></v-rating>
               <v-btn color="primary" @click="payment"
                  >Pay</v-btn>

            </v-col>
         </v-row>
      </v-form>
   </v-container>

</template>

<script>


import { db, addDoc, getDoc, doc, auth, collection,} from "@/firebase"; 
import { mapGetters } from "vuex";

export default {
	name: "pay",
   data () {
      return{
         isRented: false,
         rentedBy: "",
         IDproduct: "",
         rating: 0,
         id: this.$route.params.pid,
         seller: "",
      };
   },
   computed: {
		...mapGetters({
			user: "user",
		}),

	},
	mounted() {
		this.proizvod();
    //  this.getsellerid();
   },

methods: {
    async payment() {
            const Payment = {
				isRented: true,
            rentedBy: this.user.uid,
            IDproduct:  this.id,
            seller: this.seller,
            rating: this.rating,
				};
         const docRef = await addDoc(collection(db, "payment"), Payment);

         console.log("document writen with id: ", docRef.id)
      
      },

         async proizvod(){
         const docRef = doc(db, "products", this.id);
			const docSnap = await getDoc(docRef);
			this.product = docSnap.data();
        
         },
           /*  async getsellerid (){
               if (this.IDproduct = this.id) {
               const docRef = doc(db, "products", "uid");
               const docSnap = await getDoc(docRef);
               console.log(docSnap)
            }  else {
               
               console.log("No such document!");
         }
      },            */
   }      
};
      

</script>

<style></style>
