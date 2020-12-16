<template>
    <v-container>
        <h2 class="orange--text font-weight-bold  mb-3">
          Finding your energy
        </h2>
        <v-row v-for="(r, keyr) in rowCount" :key="keyr" >
            <v-col v-for="(c, keyc) in colNumber" :key="keyc">
                <div @click="selectPicture(keyr*colNumber+keyc)">
                    <v-card outlined :color="getColor(keyr*colNumber+keyc)">
                <v-img
                    :src="items[keyr*colNumber+keyc]"
                    aspect-ratio="1"
                    class="grey lighten-2"
                    height="pixshape"
                    width="pixshape"
                >
                    <template v-slot:placeholder>
                    <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                    >
                        <v-progress-circular indeterminate color="grey lighten-5" />
                    </v-row>
                    </template>
                </v-img>
                    </v-card>
                </div>
            </v-col>
        </v-row>
        <v-layout>

        <v-flex xs12 sm6 md12 text-center my-5><v-btn
                        @click="clearSelect"
                    color="red">deselect</v-btn></v-flex>
        <v-flex xs12 sm6 md12 text-center my-5><v-btn
                        @click="get_gifs2"
                    color="orange">refresh</v-btn></v-flex>

        <v-flex xs12 sm6 md12 text-center my-5><v-btn
                        @click="clearAndRefresh"
                    color="green">find neighbor</v-btn></v-flex>

        </v-layout>
    </v-container>
</template>


<script>
  import crypto from 'crypto';
  const shasum = crypto.createHash('sha1');
  export default {
    name: 'RandomGet',
    data(){
        return {
            dataUrl:"",
            items:[],
            selected: -1,
            latent:[],
            r1 : 0.25,
            r2 : 0.5,
        }
    },
    
    mounted() {
        this.axios.get("https://c7c273c3f0db.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = []
            for (let step = 0; step < this.colNumber*this.rowCount; step++){
                shasum.update((new Date).toString());
                this.axios.get("https://c7c273c3f0db.ngrok.io/generate_gif2?hash=" + String(shasum.digest('hex')))
                .then((response) => {
                    const prefix = `data:image/png;base64,`;
                    const base64 = response.data.img;
                    this.dataUrl = prefix + base64;
                    this.items.push(prefix + base64);
                    this.latent.push(response.data.latent)
                })
                .catch((e) => {
                    alert(e);
                });
        }
        }).catch(() => {
            alert("back end server is off-line \n we will show pre-rendered animation");
            this.items = [require("@/assets/000000.gif"),
                require("@/assets/000001.gif"),
                require("@/assets/000002.gif"),
                require("@/assets/000003.gif"),
                require("@/assets/000004.gif"),
                require("@/assets/000005.gif"),
                require("@/assets/000006.gif"),
                require("@/assets/000007.gif"),
                require("@/assets/000008.gif"),
                require("@/assets/000009.gif"),
                require("@/assets/000010.gif"),
                require("@/assets/000011.gif"),
                require("@/assets/000012.gif"),
                require("@/assets/000013.gif"),
                require("@/assets/000014.gif"),
                require("@/assets/000015.gif"),
                require("@/assets/000016.gif"),
                require("@/assets/000017.gif"),
                require("@/assets/000018.gif"),
                require("@/assets/000019.gif"),
                require("@/assets/000020.gif"),
                require("@/assets/000021.gif"),
                require("@/assets/000022.gif"),
                require("@/assets/000023.gif"),
                require("@/assets/000024.gif"),
                require("@/assets/000025.gif"),
                require("@/assets/000026.gif"),
                require("@/assets/000027.gif"),
                require("@/assets/000028.gif"),
                require("@/assets/000029.gif"),
                require("@/assets/000030.gif"),
                require("@/assets/000031.gif"),
                require("@/assets/000032.gif"),
                require("@/assets/000033.gif"),
                require("@/assets/000034.gif"),
                require("@/assets/000035.gif"),
                require("@/assets/000036.gif"),
                require("@/assets/000037.gif"),
                require("@/assets/000038.gif"),
                require("@/assets/000039.gif"),
                require("@/assets/000040.gif"),
                require("@/assets/000041.gif"),
                require("@/assets/000042.gif"),
                require("@/assets/000043.gif"),
                require("@/assets/000044.gif"),];

        });
    },

    computed: {
      colNumber: function() {
        let number;
        switch (this.$vuetify.breakpoint.name) {
          case 'xs': number = 3; break;
          case 'sm': number = 3; break;
          case 'md': number = 5; break;
          case 'lg': number = 7; break;
          case 'xl': number = 7; break;
        }
        return number;
      },

      tailSize: function() {
        let number;
        switch (this.$vuetify.breakpoint.name) {
          case 'xs': number = 300; break;
          case 'sm': number = 300; break;
          case 'md': number = 300; break;
          case 'lg': number = 300; break;
          case 'xl': number = 300; break;
        }
        return number;
      },

      pixshape: function() {
        return Math.ceil(this.$vuetify.breakpoint.width / this.colNumber);
      },

      rowCount:function(){
        return Math.ceil((this.$vuetify.breakpoint.height - this.tailSize)  / this.pixshape);
      },
    },

    methods: {

      get_gifs2:function() {
        this.axios.get("https://c7c273c3f0db.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = []
            for (let step = 0; step < this.colNumber*this.rowCount; step++){
                shasum.update((new Date).toString());
                this.axios.get("https://c7c273c3f0db.ngrok.io/generate_gif2?hash=" + String(shasum.digest('hex')))
                .then((response) => {
                    const prefix = `data:image/png;base64,`;
                    const base64 = response.data.img;
                    this.dataUrl = prefix + base64;
                    this.items.push(prefix + base64);
                    this.latent.push(response.data.latent);
                })
                .catch((e) => {
                    alert(e);
                });
        }
        }).catch(() => {
            alert("back end server is off-line \n no refresh");
        });
        this.r1 = 0.25
        this.r2 = 0.5
      },

      get_gifs3:function() {
        this.axios.get("https://c7c273c3f0db.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = [];
            const _latent = (this.selected == -1) ? [] : this.latent[this.selected];
            const params = new URLSearchParams();
            params.append('latent', _latent)
            params.append('r1', this.r1)
            params.append('r2', this.r2)


            this.latent = []

            for (let step = 0; step < this.colNumber*this.rowCount; step++){
                shasum.update((new Date).toString());

                this.axios.post("https://c7c273c3f0db.ngrok.io/generate_gif3?hash=" + String(shasum.digest('hex')), 
                params)
                .then((response) => {
                    const prefix = `data:image/png;base64,`;
                    const base64 = response.data.img;
                    this.dataUrl = prefix + base64;
                    this.items.push(prefix + base64);
                    this.latent.push(response.data.latent)
                })
                .catch((e) => {
                    alert(e);
                });
        }
        }).catch(() => {
            alert("back end server is off-line \n no refresh");
        });
        this.r1 = this.r1 * (3/4)
        this.r2 = this.r2 * (1/4)
      },

      selectPicture:function(id){
          this.selected = id
      },

      clearSelect:function(){
          this.selected = -1
      },

      getColor:function(id){
          if (id == this.selected) return 'orange'
          else return 'white'
      },
      
      selected_id:function(id){
          if (id == this.selected) return true
          else return false
      },

      clearAndRefresh:function(){
          this.get_gifs3()
      },

      itemCountInRow:function(row){
        return this.attendances.slice((row - 1) * this.colNumber, row * this.colNumber)
      }
    }
  }
</script>


<style scoped lang="scss">
$outline-width: thick;
</style>