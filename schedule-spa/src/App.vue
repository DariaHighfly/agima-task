<template>
  <div class="main">
    <div class="schedule">
      <div class="time">
        <div class="transfer">
          <img class="transfer__img" src="./assets/star.png" alt="drag-image"/>
        </div>
        <div class="timetable">
          <table class="timetable__table">
            <tr class="timetable__title">
              <th class="timetable__title_item">Время</th>
              <th class="timetable__title_item">Событие</th>
              <th></th>
            </tr>
            <tbody name="addToList" is="transition-group">
              <tr class="timetable__list"
                  v-for="item in scheduleList[currentDay]" :key="item.id">
                <td>
                  <textarea v-model="item.time" class="timetable__list_time"></textarea>
                </td>
                <td>
                  <textarea v-model="item.event" class="timetable__list_name"></textarea>
                </td>
                <td>
                  <img class="timetable__list_delete"
                       src="./assets/delete.png"
                       alt="delete-image"
                       v-on:click="deleteEvent(item.id)"
                  />
                </td>
              </tr>
            </tbody>
            <tr class="timetable_add">
              <td><input v-model="newTime"
                         class="timetable__input_time"
                         v-on:keyup.enter="addEvent">
              </td>
              <td><input v-model="newEvent"
                         class="timetable__input_name"
                         v-on:keyup.enter="addEvent">
              </td>
              <td>
                <img class="timetable__list_add"
                     src="./assets/plus.png"
                     alt="delete-image"
                     v-on:click="addEvent"
                />
              </td>
            </tr>
          </table>
        </div>
      </div>
      <div class="week-day">
        <ul class="week-day__list">
          <li v-for="day in daysOfWeek"
              :key="day.key"
              class="week-day__list_item"
              :class="{'selected-day ' : day.key === currentDay}"
              v-on:click="changeDay(day.key)">{{day.weekDay}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      currentDay: ((new Date()).getDay() === 0) ? 6 : (new Date()).getDay() - 1,
      newTime: "",
      newEvent: "",
      daysOfWeek: [
        {
          key: 0,
          weekDay: "Пн"
        },
        {
          key: 1,
          weekDay: "Вт"
        },
        {
          key: 2,
          weekDay: "Ср"
        },
        {
          key: 3,
          weekDay: "Чт"
        },
        {
          key: 4,
          weekDay: "Пт"
        },
        {
          key: 5,
          weekDay: "Сб"
        },
        {
          key: 6,
          weekDay: "Вс"
        }
      ],
      scheduleList: [
              [
                {
                  id: 0,
                  time: "10:00",
                  event: "Обед"
                },
                {
                  id: 1,
                  time: "11:00",
                  event: "Обед"
                },
                {
                  id: 2,
                  time: "12:00",
                  event: "Обед"
                },
                {
                  id: 3,
                  time: "13:00",
                  event: "Обед"
                },
              ],
              [
                {
                  id: 0,
                  time: "10:00",
                  event: "Обед"
                },
              ],
              [],
              [],
              [
                {
                  id: 0,
                  time: "15:00",
                  event: "Залить код"
                },
                {
                  id: 1,
                  time: "19:00",
                  event: "Сдать тестовое задание, чтобы попасть на стажировку"
                }
              ],
              [
                {
                  id: 0,
                  time: "20:00",
                  event: "Ужин"
                },
              ],
              []
      ]
    }
  },
  computed: {
    maxId: function () {
      return [this.scheduleList[0].length,
        this.scheduleList[1].length,
        this.scheduleList[2].length,
        this.scheduleList[3].length,
        this.scheduleList[4].length,
        this.scheduleList[5].length,
        this.scheduleList[6].length];
    }
  },
  methods: {
    addEvent() {
      if (this.newTime !== "" && this.newEvent !== "") {
        this.scheduleList[this.currentDay].push(
                {
                  id: this.maxId[this.currentDay],
                  time: this.newTime,
                  event: this.newEvent
                }
        );
      }
      this.scheduleList[this.currentDay].sort((a,b) => a.time.localeCompare(b.time));
      console.log(this.scheduleList[this.currentDay]);
      this.newTime = "";
      this.newEvent = "";
    },
    deleteEvent(id) {
      let index = this.scheduleList[this.currentDay].findIndex((elem) => elem.id === id);
      this.scheduleList[this.currentDay].splice(index, 1);
    },
    changeDay(key) {
      this.currentDay = key;
    }
  },
  mounted() {
    if (localStorage.newTime) {
      this.newTime = localStorage.newTime;
    }
  },
}
</script>

<style>
  @font-face {
    font-family: "Gilroy";
    src: url('assets/Gilroy-Regular.eot');
  }
  body {
    padding: 0;
    margin: 0;
  }
  .main {
    font-family: "Gilroy", sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f5f7f7;
    height: 100vh;
  }
  .schedule {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: flex-start;
    background-color: #ffffff;
    width: 500px;
    height: 600px;
    padding:50px;
    /*border: 3px solid #01ac6a;*/
    border-radius: 5px;
    box-shadow: 10px 10px 14px -3px rgba(0,0,0,0.05);
  }
  .transfer {
    display: flex;
    justify-content: flex-end;
    margin: 10px;
  }
  .transfer__img {
    width: 25px;
  }
  .transfer__img:hover {
    cursor: move;
  }
  .time {
    display: flex;
    flex-direction:column;
    margin-right: 40px;
    box-shadow: 4px 4px 13px 10px rgba(0,0,0,0.03);
    border-radius: 5px;
    width: 100%;
    min-height: 80%;
  }
  .timetable {
    display: flex;
    justify-content: flex-end;
    padding: 30px 30px 0 50px;
  }
  table {
    table-layout: auto;
    width: 100%;
  }
  table th, table td {
    text-align: left;
    vertical-align: top;
  }
  table th {
    font-size: 16px;
    font-weight: normal;
    padding-bottom: 20px;
  }
  .timetable__list_time, .timetable__list_name {
    outline: none;
    border: none;
    resize: none;
    font-family: "Gilroy", sans-serif;
    font-size: 14px;
    font-weight: normal;
  }
  .timetable__list_time {
    width: 50px;
    font-size: 16px;
    font-weight: bold;
    color: #01ac6a;
  }
  .timetable__list_name {
    width: 160px;
    min-height: 60px;
  }
  .timetable__list_delete, .timetable__list_add {
    margin-left: 30px;
    padding: 0;
  }
  .timetable__list_delete:hover, .timetable__list_add:hover {
    cursor: pointer;
  }
  .timetable__list_delete {
    width: 15px;
    height: 15px;
  }
  .timetable__list_add {
    margin: 30px 0 0 20px;
    width: 30px;
    height: 30px;
  }

  /***ANIMATION***/

  .addToList-enter-active {
    transition: all 0.4s;
  }
  .addToList-enter {
    opacity: 0;
  }

  /***ANIMATION***/

  .timetable__input_time, .timetable__input_name {
    border: 2px solid #f0f2f2;
    border-radius: 4px;
    margin: 30px 0 0 0;
    resize: none;
    outline: none;
  }
  .timetable__input_time {
    width: 60px;
    height: 30px;
  }
  .timetable__input_name {
    width: 160px;
    height: 30px;
  }
  .week-day {
    background-color: #f5f7f7;
  }
  .week-day__list {
    display: flex;
    flex-direction:column;
    justify-content: center;
    align-items: center;
    list-style-type: none;
    padding: 0;
    margin: 0;
    font-size: 16px;
  }
  .week-day__list_item {
    padding: 10px 30px 10px 30px;
  }
  .week-day__list_item:hover {
    background-color: #01ac6a;
    cursor: pointer;
  }
  .selected-day {
    background-color: #01ac6a;
  }
</style>
