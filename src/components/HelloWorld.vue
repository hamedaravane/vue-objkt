<template>
  <ul class="hello">
    <li v-for="rec of records" :key="rec.id" style="display: block">
      {{ rec.event_type }}
    </li>
  </ul>
</template>


<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      records: [],
    };
  },

  created() {
    let gqlBody = {
      query: `
    query MyQuery {
        event {
          marketplace_event_type
          event_type
          amount
          creator {
            address
            alias
            email
            ethereum
          }
        }
      }
      
    `,
    };

    axios
      .post("https://data.objkt.com/v3/graphql", gqlBody)
      .then((response) => {
        console.log(response);
        this.records = response.data.data.event;
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
