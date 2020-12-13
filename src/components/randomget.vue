<template>
<div>
<div id="message_board">
  <div v-for="(item, key) in items" :key="key">
    <div>
      <img v-bind:src="item"/>
    </div>
    
  </div>
  <button v-on:click="getIp"> update </button>
</div>

</div>
</template>


<script>
  export default {
    name: 'Randomget',
    data(){
        return {
            dataUrl:"",
            items:[]
        }
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
        }

    }
  }
</script>