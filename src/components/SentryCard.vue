<template>
  <div class="p-3">
    <div class="card w-lg-50 w-sm-100 offset-lg-6 bg-transperent mb-4 mr-3 shadow-bg">
      <div class="card-header text-center">
        <h1 class="text-uppercase">Sentery App</h1>
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
              <input type="time" id="name" v-model="endTime" class="form-control" />
            </div>
          </div>
          <div class="col-12 my-2">
            <form action="">
              <div class="form-group row">
                <div class="col-lg-10 col-sm-11">
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

                <div class="col-lg-2 col-sm-12">
                  <button
                    type="button"
                    class="form-control btn-success btn-sm"
                    @click.prevent="savePersonnel"
                  >
                    add
                  </button>
                </div>
                <div class="col-lg-4 offset-lg-4 offset-sm-6 mt-3 col-sm-6">
                  <button
                    ref="btn"
                    type="button"
                    class="form-control btn-outline-cstm-secondary"
                    @click.prevent="distributeTime"
                  >
                  <span  v-show="!svgLoader">Sentry time</span>
                    
                    <span>
                      <img v-show="svgLoader" src="../assets/Spinner-200px.svg" height="30" alt="">
                    </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <table class="table table-responsive table-hover">
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
              class="t-row"
            >
              <td>{{ ++index }}</td>
              <td scope="row">{{ personnel.name }}</td>
              <td>{{ personnel.end + " - " + personnel.start }}</td>
              <td class="text-danger t-data">
                <svg
                  width="1em"
                  height="1em"
                  viewBox="0 0 16 16"
                  class="bi bi-x svg"
                  fill="currentColor"
                  xmlns="http://www.w3.org/2000/svg"
                  @click="deletePersonnel(index)"
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
// elem.show();
 
export default {
  name: "SentryCard",
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
      svgLoader:false
    };
  },
  methods: {
    deletePersonnel: function(index) {

    
       
      if (index > -1) {
        index--;
        this.sentryPersonnels.splice(index, 1);
        console.log(index);
      }
    },

    savePersonnel: function() {
      this.sentryPersonnels.push({
        name: this.names,
        start: "-:-",
        end: "-:-",
      });

      this.names = "";
      // console.log(this.sentryPersonnels);
      // console.log(this.startTime);
      // console.log(this.$refs.btn);
      // console.log(this.displayMode);
    },

    // verlidate: function(){
    //   if (this.sentryPersonnels.length==0) {
        
      
           
    //   }
      
    // },

    distributeTime: function() {

     

        this.svgLoader=true
     setTimeout(() => {
        this.svgLoader=false
    
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
        if (startSeconds > this.totalTimeInMinutes) {
          this.totalTimeInMinutes = this.totalTimeInMinutes + ((24*60 ) - startSeconds);
        }
        else{
           this.totalTimeInMinutes = this.totalTimeInMinutes - startSeconds;
        }
     
      
      let secondsPerPerson = Math.floor(
        this.totalTimeInMinutes / this.sentryPersonnels.length
      );
      // console.log();
      let initialHour = this.startTime[0];
      // let minutesleft = 0;
      let personelSentryhour = initTime;
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
                  if (initialHour >= 24) {
              initialHour = 0;
            }
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

        if (secondsPerPerson >= 60) {
          //  personelSentryhour =initTime
           currentMinute = personelSentryhour.split(":");

            currentMinute = Number(currentMinute[1]) + Number(secondsPerPerson);
          // currentMinute = secondsPerPerson;
          if (secondsPerPerson >= 60) {
            do {
              initialHour++;
              currentMinute = currentMinute - 60;

               if (initialHour >= 23) {
              initialHour = "00";
            }

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
      console.log(secondsPerPerson);
      // return this.sentryTime;
        }, 1500);
    },
  },

  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table.table .svg {
  visibility: hidden;
}
table.table tr:hover {
  cursor: pointer;
  box-shadow: 0 1px 6px 0 rgba(32, 33, 36, 0.28);
}
table.table tr:hover .svg {
  visibility: visible;
  cursor: pointer;
}

h1 {
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
}
.card {
  background-color: rgba(240, 236, 21, 0.938) !important;
}
.ffset-6 {
  offset: 5em;
}
input{
  background-color: rgba(245, 245, 245, 0.719);
}
.btn-outline-cstm-secondary{
  background-color: rgb(63,61,86);
  color: whitesmoke;
  outline: none;
  border: none;
}
.btn-outline-cstm-secondary:hover{
    background-color: rgba(63, 61, 86, 0.904);
    background-color: gradient();
}
</style>
