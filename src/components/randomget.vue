<template>
    <v-container>
        <h2 class="orange--text font-weight-bold  mb-3">
          Select your energy
        </h2>
        <v-row v-for="(r, keyr) in rowCount" :key="keyr" >
            <v-col v-for="(c, keyc) in colNumber" :key="keyc">
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
            </v-col>
        </v-row>
        <v-flex xs12 sm6 md12 text-center my-5><v-btn
                        @click="get_gifs"
                    color="orange">refresh</v-btn></v-flex>
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
            items:[]
        }
    },
    
    mounted() {
        this.axios.get("https://c7c273c3f0db.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = []
            for (let step = 0; step < this.colNumber*this.rowCount; step++){
                shasum.update((new Date).toString());
                //this.axios.get("http://localhost:15000/generate_gif", 
                this.axios.get("https://c7c273c3f0db.ngrok.io/generate_gif?hash=" + String(shasum.digest('hex')), 
                {responseType: "arraybuffer"})
                .then((response) => {
                    const prefix = `data:${response.headers["content-type"]};base64,`;
                    const base64 = new Buffer(response.data, "binary").toString("base64");
                    //this.dataUrl = prefix + response.data.img;
                    //this.items.push(prefix + response.data.img);
                    this.dataUrl = prefix + base64;
                    this.items.push(prefix + base64);
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
                require("@/assets/000014.gif"),];

        });
    },

    computed: {
      colNumber: function() {
        let number;
        switch (this.$vuetify.breakpoint.name) {
          case 'xs': number = 3; break;
          case 'sm': number = 4; break;
          case 'md': number = 5; break;
          case 'lg': number = 6; break;
          case 'xl': number = 7; break;
        }
        return number;
      },

      pixshape: function() {
        return Math.ceil(this.$vuetify.breakpoint.width / this.colNumber);
      },

      rowCount:function(){
        return Math.ceil((this.$vuetify.breakpoint.height - 300)  / this.pixshape);
      },
    },

    methods: {
      getIp: function() {
        this.items = []
        const params = new URLSearchParams();
        params.append('num', 3);
        this.axios.post('https://c970cc9ad36a.ngrok.io/generate', params)
            .then((response) => {
                const prefix = `data:image/png;base64,`;
                //const base64 = new Buffer(response.data.img, "binary").toString("base64");
                for (let ret_img in response.data.img) {
                    this.dataUrl = prefix + response.data.img[ret_img];
                    this.items.push(prefix + response.data.img[ret_img]);
                }

                console.log(response.data.img);
                //console.log(response.data.img);
            })
            .catch((e) => {
                alert(e);
            });
      },

      get_gifs:function() {
        this.axios.get("https://c7c273c3f0db.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = []
            for (let step = 0; step < this.colNumber*this.rowCount; step++){
                shasum.update((new Date).toString());
                //this.axios.get("http://localhost:15000/generate_gif", 
                this.axios.get("https://c7c273c3f0db.ngrok.io/generate_gif?hash=" + String(shasum.digest('hex')), 
                {responseType: "arraybuffer"})
                .then((response) => {
                    const prefix = `data:${response.headers["content-type"]};base64,`;
                    const base64 = new Buffer(response.data, "binary").toString("base64");
                    //this.dataUrl = prefix + response.data.img;
                    //this.items.push(prefix + response.data.img);
                    this.dataUrl = prefix + base64;
                    this.items.push(prefix + base64);
                    console.log(this.dataUrl);
                })
                .catch((e) => {
                    alert(e);
                });
        }
        }).catch(() => {
            alert("back end server is off-line \n no refresh");
        });
      },
      itemCountInRow:function(row){
        return this.attendances.slice((row - 1) * this.colNumber, row * this.colNumber)
      }
    }
  }
</script>