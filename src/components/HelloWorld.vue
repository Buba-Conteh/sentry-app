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
              <div class="form-group row">
                <div class="col-10">
                  <!-- <label for="">Personnels Name</label> -->
                  <input
                    placeholder="Add personel name here"
                    type="text"
                    v-model="names"
                    id=""
                    class="form-control"
                  />
                  <!-- <span class="text-danger">Please make sure you enter personnlls first</span> -->
                </div>

                <div class="col-2">
                  <button
                    type="button"
                    class="form-control btn-success btn-sm"
                    @click.prevent="savePersonnel"
                  >
                    add
                  </button>
                </div>
                <div class="col-4 offset-4 mt-3">
                  <button
                    type="button"
                    class="form-control btn-outline-warning"
                    @click.prevent="distributeTime"
                  >
                    Sentry time
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <table class="table table-responsive">
          <thead class="thead-inverse">
            <tr>
              <th>#</th>
              <th>Name</th>
              <th>Time</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="(personnel, index) in sentryPersonnels"
              :key="index"
               @mouseover="showTrash()"
               @mouseout="hideTrash()"
            >
              <td>{{ ++index }}</td>
              <td scope="row">{{ personnel.name }}</td>
              <td>{{ personnel.end + " - " + personnel.start }}</td>
              <td class="text-danger">
                <svg
                 
                  width="1em"
                  height="1em"
                  viewBox="0 0 16 16"
                  class="bi bi-x"
                  fill="currentColor"
                  xmlns="http://www.w3.org/2000/svg"

                v-bind:style="{ display: displayMode }"
                 
                >
                  <path
                    fill-rule="evenodd"
                    d="M11.854 4.146a.5.5 0 0 1 0 .708l-7 7a.5.5 0 0 1-.708-.708l7-7a.5.5 0 0 1 .708 0z"
                  />
                  <path
                    fill-rule="evenodd"
                    d="M4.146 4.146a.5.5 0 0 0 0 .708l7 7a.5.5 0 0 0 .708-.708l-7-7a.5.5 0 0 0-.708 0z"
                  />
                </svg>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
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
      displayMode:'none'
    };
  },
  methods: {

    showTrash: function () {
      // this.$refs.trash.style.diplay :'inline'
      this.displayMode="inline"
      console.log(this.displayMode);
    },
    hideTrash: function () {
      // this.$refs.trash.style.diplay :'inline'
      this.displayMode="none"
      // console.log(this.displayMode);
    },
    savePersonnel: function() {
      this.sentryPersonnels.push({
        name: this.names,
        start: "-:-",
        end: "-:-",
      });

      this.names = "";
      console.log(this.sentryPersonnels);
      console.log(this.startTime);
      // console.log(this.$refs.trash);
      console.log(this.displayMode);

     
    },
   
   
    distributeTime: function() {
      // this.sentryPersonnels = [];
      let initTime = this.startTime;
      this.startTime = this.startTime.split(":");
      this.endTime = this.endTime.split(":");

      let startSeconds =
        Number(this.startTime[0] * 60) + Number(this.startTime[1]);
      this.totalTimeInMinutes =
        Number(this.endTime[0] * 60) + Number(this.endTime[1]);
      if (this.sentryPersonnels.length - 1 < 1) {
        this.error.massage = "Please make sure you insert personels first";
      }

      this.totalTimeInMinutes = this.totalTimeInMinutes - startSeconds;

      let secondsPerPerson = Math.floor(
        this.totalTimeInMinutes / this.sentryPersonnels.length
      );

      let initialHour = this.startTime[0];
      // let minutesleft = 0;
      let personelSentryhour = "";
      let currentMinute;

      this.sentryPersonnels.forEach((personnel, index) => {
        console.log(personnel);

        if (secondsPerPerson < 60) {
          if (index == 0) {
            personelSentryhour = initialHour + ":" + secondsPerPerson;
            this.sentryTime.push(personelSentryhour);
            this.sentryPersonnels[index] = {
              name: personnel.name,
              start: personelSentryhour,
              end: initTime,
            };
          }

          if (index > 0) {
            currentMinute = personelSentryhour.split(":");

            currentMinute = Number(currentMinute[1]) + Number(secondsPerPerson);
            if (currentMinute >= 60) {
              do {
                initialHour++;
                currentMinute = currentMinute - 60;
              } while (currentMinute >= 60);
            }

            initialHour =
              ("" + initialHour).length == 1
                ? "0" + ("" + initialHour)
                : initialHour;
            currentMinute =
              ("" + currentMinute).length == 1
                ? "0" + ("" + currentMinute)
                : currentMinute;

            personelSentryhour = initialHour + ":" + currentMinute;
            this.sentryTime.push(personelSentryhour);
            initTime = this.sentryPersonnels[index - 1]["start"];

            this.sentryPersonnels[index] = {
              name: personnel.name,
              start: personelSentryhour,
              end: initTime,
            };
          }
        }

        if (secondsPerPerson > 60) {
          currentMinute = secondsPerPerson;
          if (currentMinute >= 60) {
            do {
              initialHour++;
              currentMinute = currentMinute - 60;
            } while (currentMinute >= 60);

            if (initialHour >= 24) {
              initialHour = "00";
            }
          }

          initialHour =
            ("" + initialHour).length == 1
              ? "0" + ("" + initialHour)
              : initialHour;
          currentMinute =
            ("" + currentMinute).length == 1
              ? "0" + ("" + currentMinute)
              : currentMinute;

          personelSentryhour = initialHour + ":" + currentMinute;
          this.sentryTime.push(personelSentryhour);
          if (index - 1 < 0) {
            this.sentryPersonnels[index] = {
              name: personnel.name,
              start: personelSentryhour,
              end: initTime,
            };
          } else {
            initTime = this.sentryPersonnels[index - 1]["start"];

            this.sentryPersonnels[index] = {
              name: personnel.name,
              start: personelSentryhour,
              end: initTime,
            };
          }
        }
      });

      // this.sentryPersonnels.map
      //    initialHour+=secondsPerPerson

      console.log(this.startTime, startSeconds);
      // console.log(this.endTime, this.totalTimeInMinutes);
      console.log(this.sentryPersonnels);
      console.log(this.sentryTime);
      // return this.sentryTime;
    },
  },

  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

/* .t-row:hover ~ svg{
   display: inline;
   cursor: pointer;
 } */
.t-row:hover {
  
  cursor: pointer;
}
</style>
