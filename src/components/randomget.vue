<template>
  <v-row>
    <v-col
      v-for="n in 12"
      :key="n"
      class="d-flex child-flex"
      cols="3"
    >
      <v-img
        :src="items[n-1]"
        aspect-ratio="1"
        class="grey lighten-2"
      >
        <template v-slot:placeholder>
          <v-row
            class="fill-height ma-0"
            align="center"
            justify="center"
          >
            <v-progress-circular
              indeterminate
              color="grey lighten-5"
            ></v-progress-circular>
          </v-row>
        </template>
      </v-img>
    </v-col>
  </v-row>
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
        this.axios.get("https://c970cc9ad36a.ngrok.io/wakeup_test"
        ).then(() => {
            this.items = []
            for (let step = 0; step < 16; step++){
                shasum.update((new Date).toString());
                //this.axios.get("http://localhost:15000/generate_gif", 
                this.axios.get("https://c970cc9ad36a.ngrok.io/generate_gif?hash=" + String(shasum.digest('hex')), 
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
                    console.log(response.data.img[ret_img]);
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

      get_gifs: function() {
        this.items = []
        // const params = new URLSearchParams();
        this.axios.get('https://c970cc9ad36a.ngrok.io/generate_gif')
            .then((response) => {
                const prefix = `data:image/png;base64,`;
                //const base64 = new Buffer(response.data.img, "binary").toString("base64");
                for (let ret_img in response.data.img) {
                    console.log(response.data.img[ret_img]);
                    this.dataUrl = prefix + response.data.img[ret_img];
                    this.items.push(prefix + response.data.img[ret_img]);
                }

                console.log(response.data.img);
                //console.log(response.data.img);
            })
            .catch((e) => {
                alert(e);
            });
        }



    }
  }
</script>