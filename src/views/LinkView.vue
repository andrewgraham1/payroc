<template>
  <div class="main">
    <div class="content">
      <h2>Shorten URLs here</h2>
      <input v-model="text" placeholder="URL here" />
      <button @click="submit">Submit</button>
      <p class="response">Response: {{ info }}</p>
      <p>
        By clicking SUBMIT, you are agreeing to DINOCO's Terms of Service,
        Privacy Policy, and Acceptable Use Policy
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      text: "",
      response: "",
      url: {
        post: "http://localhost:3001/shorten-url",
      },
      info: "URL must start with either http:// or https://",
    };
  },
  methods: {
    async submit() {
      const regEx = new RegExp("^(http|https)://");
      if (regEx.test(this.text)) {
        try {
          const response = await axios({
            method: "post",
            url: this.url.post,
            headers: {
              "Content-Type": "application/json",
            },
            data: { text: this.text },
          });
          this.info = response.data;
          console.log(this.info);
        } catch (myError) {
          console.log(myError);
          console.log("Unsuccesfful");
          this.info = "";
        }
        return;
      } else {
        console.log("no http:// found");
        this.info = "URL must start with either http:// or https://";
        //error for user not using http:// https:// here
      }
    },
  },
};
</script>

<style>
.response {
  font-weight: bold;
  font-size: larger;
}

h2 {
  font-weight: bold;
  font-size: 30px;
}
button {
  background-color: white; /* Green */
  border: 2px solid black;
  color: black;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border-radius: 5px;
  transition-duration: 0.4s;
  margin: 10px;
  font-weight: bold;
}
button:hover {
  box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24),
    0 17px 50px 0 rgba(0, 0, 0, 0.19);
  background-color: lightblue; /* Green */
  color: black;
}
button:active {
  background-color: red; /* Green */
  color: black;
}

input {
  height: 50px;
  width: 500px;
  border-radius: 15px;
  padding: 10px;
  margin: 10px;
  border: 2px solid black;
  color: black;
}

.main {
  background-color: darkblue;
  width: 70%;
  height: 450px;
  border-radius: 35px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.content {
  background-color: lightskyblue;
  width: 97.5%;
  height: 95%;
  bottom: 2.5%;
  border-radius: 25px;
  padding: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: black;
}
</style>
