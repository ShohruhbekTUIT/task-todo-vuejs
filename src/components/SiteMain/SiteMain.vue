<script>
import Item from '../Item/Item.vue';
import SiteFooter from '../SiteFooter/SiteFooter.vue';
let bugun = [];
let ertaga = [];
let keyin = [];

const regex = /(\d{1,2})\.(\d{1,2})\.(\d{4})/;
const regextime = /\b([01]?[0-9]|2[0-3]):[0-5][0-9]\b/;


let date = new Date();

export default {
  components: { Item, SiteFooter },


  setup() {
    let date = new Date();
    let today = `${date.getDate()}.${date.getMonth() + 1}.${date.getFullYear()}`;
    let tTime = `${date.getHours()}:${date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()}`;
    return {
      today,
      tTime,
    }
  },

  data() {
    return {
      inputVal: '',
      bugun,
      ertaga,
      keyin,
    }


  },
  methods: {
    formSubmit() {
      let elInputSort = this.inputVal.split(' ');
      if (elInputSort.includes('bugun') || elInputSort.includes('Bugun')) {
        let timeIndex = this.inputVal.match(regextime)?.index;
        let value = this.inputVal.split(' ');
        if (elInputSort[0].toLowerCase() == 'bugun') {
          elInputSort.shift();
        }
        if (this.inputVal.match(regextime)) {
          elInputSort.pop();
        }
        if (elInputSort.at(-1).toLowerCase() == 'bugun') {
          elInputSort.pop();
        }

        const newTask = {
          id: bugun[bugun.length - 1]?.id + 1 || 0,
          title: elInputSort.join(" "),
          time: this.inputVal.match(regextime)?.[0]
            ? this.inputVal.match(regextime)?.[0]
            : `${date.getHours() + 1}:00`,
          isCompleted: false,
        }

        bugun.push(newTask);
      } else if (
        elInputSort[0].toLowerCase() == 'ertaga' ||
        elInputSort.at(-1).toLowerCase() == 'ertaga' ||
        elInputSort.at(-2).toLowerCase() == 'ertaga'
      ) {
        let timeIndex = this.inputVal.match(regextime)?.index;
        let value = this.inputVal.split(' ');
        if (elInputSort[0].toLowerCase() == 'ertaga') {
          elInputSort.shift();
        }
        if (this.inputVal.match(regextime)) {
          elInputSort.pop();
        }
        if (elInputSort.at(-1).toLowerCase() == 'ertaga') {
          elInputSort.pop();
        }
        const newErtaga = {
          id: ertaga[ertaga.length - 1]?.id + 1 || 100,
          title: elInputSort.join(' '),
          time: this.inputVal.match(regextime)?.[0]
            ? this.inputVal.match(regextime)?.[0]
            : '9:00',
          isCompleted: false,
        };

        ertaga.push(newErtaga);
      } else if (this.inputVal.match(regex)) {
        let dateIndex = this.inputVal.match(regex)?.index;
        let timeIndex = this.inputVal.match(regextime)?.index;
        let value = this.inputVal.split(' ');
        if (dateIndex) {
          let result = dateIndex > 0 ? value.pop() : value.shift();
        }
        if (timeIndex) {
          let result2 = timeIndex > 0 ? value.pop() : value.shift();
        }
        const newKeyin = {
          id: keyin[keyin.length - 1]?.id + 1 || 1000,
          title: value.join(' '),
          time: `${this.inputVal.match(regex)?.[0]},${this.inputVal.match(regextime)?.[0]
            ? this.inputVal.match(regextime)?.[0]
            : '9:00'
            }`,
          isCompleted: false,
        };

        keyin.push(newKeyin);
      } else {
        const newBugun = {
          id: bugun[bugun.length - 1]?.id + 1 || 0,
          title: this.inputVal,
          time: `${date.getHours() + 1}:00`,
          isCompleted: false,
        };

        bugun.push(newBugun);
      }
      this.inputVal = '';
      // this.handleCompleted(bugun, ertaga, keyin);
      // console.log(bugun, ertaga, keyin);
    },

    handleCompleted(arr11, arr2, arr3) {
      return [...arr11, ...arr2, ...arr3].filter(i => i.isCompleted == true).length;
      // bajarilganlar = tasks.filter((el) => el.isCompleted == true).length;
      // console.log(bajarilganlar);
      // let completedId = evt.target.dataset.id;
      // console.log(evt.target.dataset.id);
      // if (completedId < 100) {
      //   let findedItem = bugun.find((item) => item.id === completedId);
      //   findedItem.isCompleted = !findedItem.isCompleted;
      //   // return findedItem;
      // }
      // if (completedId >= 100 && completedId < 1000) {
      //   let findedItem = ertaga.find((item) => item.id === completedId);
      //   findedItem.isCompleted = !findedItem.isCompleted;
      //   SetErtaga([...ertaga]);
      // }
      // if (completedId >= 1000) {
      //   let findedItem = keyin.find((item) => item.id === completedId);
      //   findedItem.isCompleted = !findedItem.isCompleted;
      //   SetKeyin([...keyin]);
      // }

      // let bajarilganArr = [...bugun, ...ertaga, ...keyin].filter(
      //   (item) => item.isCompleted === true,
      // );
      // SetBajarilganlar(bajarilganArr);
      // let bajarilmaganArr = [...bugun, ...ertaga, ...keyin].filter(
      //   (item) => item.isCompleted === false,
      // );
      // SetBajarilmaganlar(bajarilmaganArr);
    },

  },

}
</script>


<template>
  <main>
    <div>
      <form @submit.prevent="formSubmit">
        <div class="input-group">
          <label>
            <input class="site-input" type="text" placeholder="Yangi vazifa qo'shish" v-model="inputVal" required>
            Bugun: <span>{{ today + ', ' }}</span><span>{{ tTime }}</span>
          </label>
          <button type="submit">+</button>
        </div>
      </form>

      <div>
        <!-- Bugungi bajariladigan ishlar ro'yxati  -->
        <ul class="list-bugun">
          <li>
            <h2>
              {{ bugun.length > 0 ? "Bugun" : "" }}
            </h2>
          </li>
          <Item v-for='item in bugun' v-bind:item="item" v-bind:handleCompleted="handleCompleted" />
        </ul>

        <!-- Ertangi bajariladigan ishlar ro'yxati  -->
        <ul class="list-erta">
          <li>
            <h2>
              {{ ertaga.length > 0 ? "Ertaga" : "" }}
            </h2>
          </li>
          <Item v-for='item in ertaga' v-bind:item="item" />
        </ul>

        <!-- Keyin bajariladigan ishlar ro'yxati / -->
        <ul class="list-keyin">
          <li>
            <h2>
              {{ keyin.length > 0 ? "Keyin" : "" }}
            </h2>
          </li>
          <Item v-for='item in keyin' v-bind:item="item" />
        </ul>
      </div>

    </div>
  </main>
  <footer class="site-footer">
    <div class="site-footer__info">
      <strong class="site-footer__task">
        Bajarilganlar:{{ handleCompleted(bugun, ertaga, keyin) }}
      </strong>
      <strong class="site-footer__task">
        Bajarilmaganlar:{{ bugun.length + ertaga.length + keyin.length - handleCompleted(bugun, ertaga, keyin) }}
      </strong>
    </div>
  </footer>
</template>

<style scoped>
main {
  max-width: 600px;
  margin: 0 auto;
}

.input-group {
  max-width: 370px;
  display: flex;
  justify-content: space-between;
  align-items: start;
  margin: 0 auto;
  margin-bottom: 35px;
}

.site-input {
  width: 320px;
  border: 2px solid darkgray;
  border-radius: 8px;
  padding: 12px 8px;
  margin-right: 10px;
  margin-bottom: 5px;
}

label {
  color: gray;
}

button {
  background-color: rgb(62, 190, 137);
  border-color: rgb(6, 110, 67);
  border-radius: 8px;
  transition: all .5s ease;
  padding: 12px 14px;
}

button:hover {
  background-color: rgb(33, 97, 70);
  color: rgb(197, 175, 175);
}

ul {
  list-style-type: none;
}

.site-footer {
  padding: 40px;
}

.site-footer__info {
  position: absolute;
  width: 300px;
  right: 0;
  top: 500px;

}

.site-footer__task {
  display: block;
}
</style>