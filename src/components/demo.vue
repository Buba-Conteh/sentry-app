<template>
  <div>
    <div class="card w-50 m-auto">
      <div class="card-header text-center">
        Sentery App
      </div>
      <div class="card-body">
        <div class="row">
          <div class="col-12 d-flex justify-content-evenly">
            <div class="form-group ">
              <label for="">Start Time</label>
              <input
                type="time"
                v-model="startTime"
                id=""
                class="form-control"
              />
            </div>
            <div class="form-group">
              <label for="">End Time</label>
              <input type="time" v-model="endTime" class="form-control" />
            </div>
          </div>
          <div class="col-12 my-2">
            <form action="">
              <div class="form-group">
                <label for="">Personnels Name</label>
                <input type="text" v-model="names" id="" class="form-control" />
                <button type="button" @click.prevent="savePersonnel">
                  add
                </button>
                <button type="button" @click.prevent="distributeTime">
                  Sentry time
                </button>
              </div>
            </form>
          </div>
        </div>

        <table class="table table-responsive">
          <thead class="thead-inverse">
            <tr>
              <th>Name</th>
              <th>Time</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(personnel, index) in sentryPersonnels" :key="index">
              <td scope="row">{{ personnel.name }}</td>
              <td>{{ personnel.time }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "demo",
  props: {
    msg: String,
  },
  data() {
    return {
      names: "",
      startTime: "bbbb",
      endTime: "",
      sentryPersonnels: [],
      sentryTime: [],
      totalTimeInMinutes: 0,
      error: {},
    };
  },
  methods: {
    savePersonnel: function() {
      this.sentryPersonnels.push({ name: this.names, time: "" });
      this.names = "";
      console.log(this.sentryPersonnels);
      // console.log(this.startTime);
    },

    distributeTime: function() {
      this.startTime = this.startTime.split(":");
      this.endTime = this.endTime.split(":");

      let startSeconds =
        Number(this.startTime[0] * 60) + Number(this.startTime[1]);
      this.totalTimeInMinutes =
        Number(this.endTime[0] * 60) + Number(this.endTime[1]);
      if (this.sentryPersonnels.length - 1 < 1) {
        this.error.massage = "Please make sure you insert personels first";
      }
      let secondsPerPerson =
        this.totalTimeInMinutes / (this.sentryPersonnels.length);
    //   console.log("the lenght is " + this.sentryPersonnels.length);
      let initialHour = this.startTime[0];
      let minutesleft = 0;

      this.sentryPersonnels.forEach((personnel, index) => {
        // console.log(personnel);
        if (secondsPerPerson < 60) {
          this.sentryTime.push(initialHour + ":" + secondsPerPerson);
          this.sentryPersonnels[index] = {
            name: personnel.name,
            time: initialHour + ":" + secondsPerPerson,
          };
        }

        if (secondsPerPerson >= 60) {
         

            do {

            minutesleft = minutesleft - 60;
            // initialHour++;
            // console.log("whikw loop");
            console.log("looping");

            } while (minutesleft >= 60);
          
         
          

        //   this.sentryTime.push({
        //     name: personnel.name,
        //     time: initialHour + ":" + minutesleft,
        //   });

        //   this.sentryPersonnels[index] = {
        //     name: personnel.name,
        //     time: initialHour + ":" + secondsPerPerson,
        //   };
        }
      });

      // this.sentryPersonnels.map
      //    initialHour+=secondsPerPerson

      console.log(this.startTime, startSeconds);
      console.log(this.endTime, this.totalTimeInMinutes);
      console.log(secondsPerPerson);
      console.log(this.sentryTime);
      console.log(this.sentryPersonnels);
      // return this.sentryTime
    },
  },

  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
