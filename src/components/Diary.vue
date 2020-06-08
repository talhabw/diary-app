<template>
  <div class="container" v-if="$parent.login">
    <div class="searchDiary">
      <button class="logout" @click="logout">Çıkış Yap</button>
      <input type="date" @change="dateToNum" v-model="date" />
      <input type="text" v-model="search" placeholder="Write the diary title or text..." />
    </div>

    <table>
      <thead>
        <tr>
          <td></td>
          <td>
            Diary Date
            <br />(mm/dd/yyyy)
          </td>
          <td>Diary Title</td>
          <td>Diary Text</td>
        </tr>
      </thead>
      <tbody>
        <tr
          @click="diaryDetails(diary)"
          v-for="diary in filteredDiaries.slice(0, 5)"
          :key="diary.id"
        >
          <td>{{diary.id}}</td>
          <td>{{diary.date.split("-").reverse().join(" / ")}}</td>
          <td>{{diary.title}}</td>
          <td>{{diary.text.substr(0, 8)}}...</td>
        </tr>
      </tbody>
    </table>

    <div class="diary" :v-show="selectedDiary != {}">
      <h2>{{selectedDiary.id}}</h2>
      <h3>{{selectedDiary.title}}</h3>
      <h5>{{selectedDiary.date}}</h5>
      <p>{{selectedDiary.text}}</p>
    </div>

    <div class="edit">
      <!-- <input type="text" placeholder="Diary ID" value="2" disabled /> -->
      <input type="text" placeholder="Diary Title*" v-model="selectedDiary.title" required />
      <textarea cols="30" rows="10" placeholder="Diary text*" v-model="selectedDiary.text" required></textarea>
      <p>*: required</p>
      <div class="buttons">
        <button class="btn" @click="clearInputs">Vazgeç</button>
        <button class="btn" @click="saveDiary">Kaydet</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Diary",
  data() {
    return {
      diaries: [
        {
          id: 0,
          date: "2021-12-11",
          title: "title1",
          text: "text1"
        },
        {
          id: 1,
          date: "2020-02-03",
          title: "title2",
          text: "text2"
        },
        {
          id: 2,
          date: "2017-05-08",
          title: "title3",
          text: "text3"
        },
        {
          id: 3,
          date: "2021-12-12",
          title: "title4",
          text: "sss"
        }
      ],
      date: new Date().toISOString().slice(0, 10),
      dateForFilt: null,
      search: "",
      selectedDiary: {}
    };
  },
  computed: {
    filteredDiaries() {
      var self = this;
      return this.diaryDate.filter(function(diary) {
        return (
          diary.title.toLowerCase().indexOf(self.search.toLowerCase()) >= 0 ||
          diary.text.toLowerCase().indexOf(self.search.toLowerCase()) >= 0
        );
      });
      //return this.customers;
    },
    diaryDate() {
      return this.diaries.filter(diary => {
        diary.date = diary.date.split("-").map(d => parseFloat(d));

        if (diary.date[0] < this.dateForFilt[0]) {
          diary.date = diary.date.map(d => d.toString()).join("-");
          return true;
        } else if (diary.date[0] == this.dateForFilt[0]) {
          if (diary.date[1] < this.dateForFilt[1]) {
            diary.date = diary.date.map(d => d.toString()).join("-");
            return true;
          } else if (diary.date[1] == this.dateForFilt[1]) {
            if (diary.date[2] < this.dateForFilt[2]) {
              diary.date = diary.date.map(d => d.toString()).join("-");
              return true;
            } else if (diary.date[2] == this.dateForFilt[2]) {
              diary.date = diary.date.map(d => d.toString()).join("-");
              return true;
            } else {
              diary.date = diary.date.map(d => d.toString()).join("-");
              return false;
            }
          } else {
            diary.date = diary.date.map(d => d.toString()).join("-");
            return false;
          }
        } else {
          diary.date = diary.date.map(d => d.toString()).join("-");
          return false;
        }
      });
    }
  },
  methods: {
    diaryDetails(diary) {
      this.selectedDiary = diary;
    },
    clearInputs() {
      this.selectedDiary = {};
    },
    saveDiary() {
      if (
        this.selectedDiary.id === undefined &&
        this.selectedDiary.title != undefined
      ) {
        this.selectedDiary.id = this.diaries.length;
        this.selectedDiary.date = new Date().toISOString().slice(0, 10);
        this.diaries.push(this.selectedDiary);
        this.selectedDiary = {};
      } else {
        this.clearInputs();
      }
    },
    dateToNum() {
      this.dateForFilt = this.date.split("-").map(d => parseFloat(d));
    },
    logout() {
      this.$parent.login = false;
    }
  },
  created() {
    this.dateToNum();
  }
};
</script>

<style scoped>
.container {
  width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  flex-direction: column;
  flex-wrap: wrap;
}
table {
  border-collapse: collapse;
  width: 100%;
}
thead > tr {
  cursor: auto;
}
tbody > tr {
  cursor: pointer;
}
td,
th {
  border: 1px solid #d1d1d1;
  text-align: center;
  padding: 8px;
}
thead {
  background: linear-gradient(
    90deg,
    rgba(0, 255, 191, 1) 40%,
    rgba(0, 183, 255, 1) 90%
  );
}
tbody {
  background-color: #ddd;
}
.diary {
  text-align: center;
  font-family: "Courier New", Courier, monospace;
}
.logout,
.btn {
  border: none;
  border-radius: 7px;
  background: linear-gradient(
    90deg,
    rgba(0, 255, 191, 1) 40%,
    rgba(0, 183, 255, 1) 90%
  );
  outline: none;
  cursor: pointer;
  height: 36px;
  width: 215px;
  padding: 5px;
  margin-right: auto;
}
.btn {
  margin: 0;
}
.edit {
  display: flex;
  flex-direction: column;
  padding: 50px 0;
}
.edit > input {
  margin-bottom: 30px;
}
.buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}
.searchDiary {
  padding: 50px 0;
  display: flex;
  justify-content: space-between;
}
.searchDiary > input {
  margin: 0 10px;
  border: 1px solid gray;
  height: 30px;
  width: 215px;
  background: linear-gradient(
    90deg,
    rgba(0, 255, 191, 1) 40%,
    rgba(0, 183, 255, 1) 90%
  );
  transition: border 300ms linear;
  padding: 1px 5px;
  font-size: 1.1rem;
}
/* Removes the clear button from date inputs */
input[type="date"]::-webkit-clear-button {
  display: none;
}

/* Removes the spin button */
input[type="date"]::-webkit-inner-spin-button {
  display: none;
}

/* Always display the drop down caret */
input[type="date"]::-webkit-calendar-picker-indicator {
  color: #2c3e50;
}

/* A few custom styles for date inputs */
input[type="date"] {
  appearance: none;
  -webkit-appearance: none;
  color: #95a5a6;
  font-family: "Helvetica", arial, sans-serif;
  border: 1px solid gray;
  background: linear-gradient(
    90deg,
    rgba(0, 255, 191, 1) 40%,
    rgba(0, 183, 255, 1) 90%
  );
  padding: 1px 5px;
  display: inline-block !important;
  visibility: visible !important;
}

input[type="date"],
focus {
  color: #95a5a6;
  box-shadow: none;
  -webkit-box-shadow: none;
  -moz-box-shadow: none;
}
</style>