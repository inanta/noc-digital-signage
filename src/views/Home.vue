<template>
  <div class="font-body grid grid-cols-4 h-full p-4 text-secondary">
    <div class="col-span-3 overflow-hidden">
      <div class="grid grid-flow-row auto-rows-min">
        <div class="grid grid-cols-2 overflow-hidden">
          <div class="p-2 h-full overflow-hidden">
            <div class="bg-white h-full overflow-hidden rounded-md">
              <div class="grid grid-cols-7 mb-5 p-4 shadow-md text-2xl">
                <i class="fas fa-sign-in-alt self-center"></i>
                <h2 class="col-span-5 self-center text-center">Komplain</h2>
                <div>&nbsp;</div>
              </div>
              <div class="grid-grid-rows-1">
                <div
                  v-for="item in in_complaints"
                  :key="item.id"
                  class="pb-3 px-4"
                >
                  <div class="grid grid-cols-2">
                    <div class="font-bold text-2xl">{{ item.name }}</div>
                    <div class="text-right text-md">{{ item.date }}</div>
                  </div>
                  <div class="text-xl">{{ item.description }}</div>
                </div>
              </div>
            </div>
          </div>
          <div class="p-2">
            <div class="bg-white h-full rounded-md">
              <div class="grid grid-cols-7 p-4 shadow-md text-2xl">
                <i class="fas fa-sign-out-alt self-center"></i>
                <h2 class="col-span-5 self-center text-center">
                  Komplain Lapangan
                </h2>
                <div>&nbsp;</div>
              </div>
              <div v-if="out_complaints.length > 0" class="grid-grid-rows-1">
                <!-- <div v-for="item in in_complaints" :key="item.id" class="p-4">
                  <div class="grid grid-cols-2">
                    <div class="font-bold text-2xl">{{ item.name }}</div>
                    <div class="text-right text-sm">{{ item.date }}</div>
                  </div>
                  <div class="text-md">{{ item.description }}</div>
                </div> -->
              </div>
              <div v-else class="flex items-center justify-center py-5">
                <div class="overflow-hidden w-1/2">
                  <img
                    class="h-full w-full"
                    src="@/assets/images/undraw_blank_canvas_re_2hwy.svg"
                  />
                  <div class="p-2 text-xl text-center">
                    Hore! Tidak Ada Komplain Lapangan.
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="p-2">
          <div class="bg-white h-full p-5 rounded-md">
            <div class="grid grid-cols-4">
              <div>
                <div class="grid grid-rows-1 text-left">
                  <div class="font-bold self-center text-4xl">
                    {{ clockHH }}:{{ clockMM
                    }}<!-- :{{ clockSS }} -->
                  </div>
                  <div class="self-center text-3xl">
                    {{ dateDayName }}, {{ dateDay }} {{ dateMonth }}
                    <!-- {{ dateYear }} -->
                  </div>
                </div>
              </div>
              <div class="">&nbsp;</div>
              <div class="col-span-2 grid grid-rows-1">
                <div class="font-bold self-center text-4xl text-right">
                  {{ weatherTemp }}&#8451;
                </div>
                <div class="self-center text-3xl text-right capitalize">
                  <img :src="weatherIcon" class="inline-block h-8" />
                  {{ weatherStatus }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="h-full p-2">
      <div class="bg-white border border-tertiary h-full rounded-md">
        <div class="grid grid-cols-7 mb-5 p-4 shadow-md text-2xl">
          <i class="fas fa-users self-center"></i>
          <h2 class="col-span-5 self-center text-center">Tim Layanan</h2>
          <div>&nbsp;</div>
        </div>
        <div class="grid grid-rows-1">
          <div v-for="user in users" :key="user.uid" class="pb-3 px-4">
            <div class="border-b border-tertiary grid grid-cols-4 pb-4">
              <div class="relative">
                <img
                  :x-class="userPresenceStatus(user)"
                  :src="userProfilePicture(user.uid)"
                  class="border border-tertiary p-1 rounded-full w-full"
                />
                <div
                  :class="userStatusBadge(user)"
                  class="absolute border-4 border-white bottom-0 h-7 right-0 rounded-full w-7"
                ></div>
              </div>
              <div class="col-span-3 grid gridd-rows-1 pl-2">
                <div class="font-bold line-clamp-1 self-end text-2xl">
                  {{ user.name }}
                </div>
                <div class="text-xl line-clamp-2">
                  <span v-if="user.presence_out == null">
                    {{ user.status }}
                  </span>
                  <span v-else></span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    // HelloWorld,
  },
  data: function () {
    return {
      in_complaints: [
        {
          name: "Bapak Surya",
          description: "Tidak bisa internet",
          date: "05/05/2022"
        },
        {
          name: "Ibu Maria",
          description: "Tidak bisa cetak dokumen",
          date: "05/05/2022"
        }
      ],
      out_complaints: [],
      users: [
        {
          uid: 31,
          name: "Inanta Martsanto",
          status: "Pengerjaan Digital Signage",
          status_type: "0",
          presence_out: null
        },
        {
          uid: 144,
          name: "Donny Trijatmiko",
          status:
            "Konfigurasi cPanel dan server untuk deployment CI/CD aplikasi digital signage",
          status_type: 1,
          presence_out: null
        },
        {
          uid: 139,
          name: "Sherly Sanjaya",
          status_type: 1,
          presence_out: 1
        }
      ],
      dayNames: [
        "Minggu",
        "Senin",
        "Selasa",
        "Rabu",
        "Kamis",
        "Jumat",
        "Sabtu"
      ],
      monthNames: [
        "Januari",
        "Februari",
        "Maret",
        "April",
        "Mei",
        "Juni",
        "Juli",
        "Agustus",
        "September",
        "Oktober",
        "November",
        "Desember"
      ],
      dateDayName: "Minggu",
      dateDay: 1,
      dateMonth: "Januari",
      dateYear: 2000,
      clockHH: 0,
      clockMM: 0,
      clockSS: 0,
      weatherStatus: "",
      weatherTemp: 26,
      weatherIconCode: "01d",
      hash: ""
    };
  },
  mounted: function () {
    let self = this;

    self.fetchData();
    self.getWeather();

    setInterval(function () {
      self.fetchData();
    }, 3000);

    setInterval(function () {
      self.getClock();
    }, 1000);

    setInterval(function () {
      self.getWeather();
    }, 7200000);
  },
  computed: {
    weatherIcon: function () {
      return (
        "http://openweathermap.org/img/wn/" + this.weatherIconCode + "@2x.png"
      );
    }
  },
  methods: {
    userProfilePicture: function (uid) {
      return (
        "http://noc.maranatha.edu/staff/apps/ihollo/images/profile/" +
        uid +
        ".jpg"
      );
    },
    userStatusBadge: function (user) {
      return user.presence_out != null
        ? "bg-tertiary"
        : user.status_type == 0
        ? "bg-green-600"
        : "bg-red-600";
    },
    userPresenceStatus: function (user) {
      return user.presence_out == null ? "border-green-600" : "border-tertiary";
    },
    fetchData: function () {
      let self = this;

      axios({
        method: "get",
        url: "http://noc.maranatha.edu/ds-service/",
        responseType: "stream"
      }).then(function (response) {
        // console.log(response);

        if (self.hash != response.data.hash) {
          self.hash = response.data.hash;

          self.users.splice(0);

          for (let index = 0; index < response.data.presences.length; index++) {
            const presence = response.data.presences[index];

            self.users.push(presence);
          }

          self.in_complaints.splice(0);

          for (
            let index = 0;
            index < response.data.in_complaints.length;
            index++
          ) {
            const in_complaint = response.data.in_complaints[index];

            self.in_complaints.push(in_complaint);
          }
        }

        // console.log(response.data.in_complaints);
      });
    },
    getClock: function () {
      let date = new Date();

      let hh = date.getHours();
      let mm = date.getMinutes();
      let ss = date.getSeconds();

      this.dateDayName = this.dayNames[date.getDay()];
      this.dateDay = date.getDate();
      this.dateMonth = this.monthNames[date.getMonth()];
      this.dateYear = date.getFullYear();
      this.clockHH = hh < 10 ? "0" + hh : hh;
      this.clockMM = mm < 10 ? "0" + mm : mm;
      this.clockSS = ss < 10 ? "0" + ss : ss;
    },
    getWeather: function () {
      let self = this;

      axios({
        method: "get",
        url: "https://api.openweathermap.org/data/2.5/weather?lat=-6.8861&lon=107.581&lang=id&units=metric&appid=59ce474bf023e834ca54e5966dbaae0e",
        responseType: "stream"
      }).then(function (response) {
        self.weatherIconCode = response.data.weather[0].icon;
        self.weatherStatus = response.data.weather[0].description;
        self.weatherTemp = Math.round(response.data.main.feels_like);
        console.log(response.data);
      });
    }
  }
};
</script>
