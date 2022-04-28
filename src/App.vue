<script>
export default {
  data() {
    return {
      host: "?",
      ip: "?",
      location: "?",
      latency: "? ms",
    };
  },
  methods: {
    async getVariablesInfo() {
      let visitorInfos = null;
      if (localStorage.getItem("visitorInfos") == null) {
        await fetch("https://ipinfo.io/json")
          .then(async function (response) {
            visitorInfos = await response.json();
            localStorage.setItem("visitorInfos", JSON.stringify(visitorInfos));
          })
          .catch(function (error) {
            console.log(error);
          });
      } else {
        visitorInfos = JSON.parse(localStorage.getItem("visitorInfos"));
      }

      //console.log(visitorInfos);
      this.ip = visitorInfos.ip ? visitorInfos.ip : "?";
      this.location = visitorInfos.country
        ? visitorInfos.country + ", " + visitorInfos.city
        : "?";
    },
    getlatency() {
      setInterval(async () => {
        var started = new Date().getTime();
        var url = "/data.json?t=" + +new Date();
        fetch(url)
          .then(function (response) {
            var ended = new Date().getTime();
            var milliseconds = ended - started;
            document.getElementById("latency").innerHTML = milliseconds + " ms";
          })
          .catch(function (error) {
            //console.log(error);
            //clearInterval(timerLatency)
            document.getElementById("latency").innerHTML = "? ms";
          });
      }, 1000);
    },
  },
  mounted() {
    this.host = window.location.host;
    this.getVariablesInfo();
    this.getlatency();
  },
};
</script>

<template>
  <header>
    <img src="@/assets/elestio.svg" alt="logo" width="200" height="100" />
  </header>

  <main>
    <div class="app-body">
      <div class="app-heading" style="margin-bottom: 40px">
        <h1>Welcome to Elestio</h1>
        <h4>Deploy your apps quickly with the easiest CI/CD system</h4>
      </div>

      <p class="app-info-block">
        This Host <strong class="subVal" id="host">{{ host }}</strong>
      </p>

      <p class="app-info-block">
        Your IP <strong class="subVal" id="yourIP">{{ ip }}</strong>
      </p>

      <p class="app-info-block">
        Your Location
        <strong class="subVal" id="location">{{ location }}</strong>
      </p>

      <p class="app-info-block">
        Latency to server
        <strong class="subVal" id="latency">{{ latency }}</strong>
      </p>

      <div class="app-deploy">
        <a
          href="https://dash.elest.io/deploy?source=cicd&social=Github&url=https://github.com/elestio-examples/vuejs"
          class="btn mb-10-m btn-try"
          >Deploy on Elestio
        </a>
      </div>
    </div>

    <!-- BG Anim -->
    <div class="area">
      <ul class="circles">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
  </main>
</template>

<style>
@import "./assets/base.css";
</style>
