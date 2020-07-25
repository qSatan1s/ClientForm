<template>
  <div id="app">
    <form @submit.prevent="submitHandler" class="form_block">
      <div class="form_block_left">
        <h2>Регистрация</h2>
        <div class="qq">
          <div class="field">
            <input
              class="form_input"
              v-model="name"
              type="string"
              name="name"
              id="name"
              placeholder="Василий"
              :class="{invalid: ($v.name.$dirty && !$v.name.required) || ($v.name.$dirty && !$v.name.minLength)}"
            />
            <label class="form_label" for="name">
              имя
              <span class="starCheck">*</span>
            </label>
            <small
              class="helper-text invalid"
              v-if="$v.name.$dirty && !$v.name.required"
            >Введите имя</small>
            <small
              class="helper-text invalid"
              v-else-if="$v.name.$dirty && !$v.name.minLength"
            >Минимум {{$v.name.$params.minLength.min}} символа</small>
          </div>
          <div class="field">
            <input
              class="form_input"
              type="text"
              name="surname"
              v-model="surname"
              id="surname"
              placeholder="Васильев"
              :class="{invalid: ($v.surname.$dirty && !$v.surname.required)}"
            />

            <label class="form_label" for="surname">
              фамилия
              <span class="starCheck">*</span>
            </label>
            <small
              class="helper-text invalid"
              v-if="$v.surname.$dirty && !$v.surname.required"
            >Введите Фамилию</small>
          </div>
          <div class="field patronymic">
            <input
              class="form_input"
              type="text"
              v-model="middle_name"
              name="middle_name"
              id="middle_name"
              placeholder="Васильевич"
            />

            <label class="form_label" for="middle_name">Отчество</label>
          </div>

          <label class="form_label data_born" for="start">
            Дата рождения:
            <span class="starCheck">*</span>
          </label>
          <div class="field patronymic">
            <input
              class="form_input data_born"
              type="date"
              id="start"
              v-model="data_born"
              name="trip-start"
              value="2002-01-01"
              min="1940-01-01"
              max="2002-12-01"
              :class="{invalid: ($v.data_born.$dirty && !$v.data_born.required)}"
            />
            <small
              class="helper-text invalid"
              v-if="$v.surname.$dirty && !$v.surname.required"
            >Выберите дату рождения</small>
          </div>
        </div>
        <div class="field">
          <input
            type="number"
            id="phone"
            name="phone"
            v-model="phone"
            class="form_input phone"
            :class="{invalid: ($v.phone.$dirty && !$v.phone.required) || ($v.phone.$dirty && !$v.phone.minLength) || ($v.phone.$dirty && !$v.phone.phone)}"
            placeholder="791111111111"
          />
          <label class="form_label" for="phone">
            Номер телефона
            <span class="starCheck">*</span>
          </label>
          <small
            class="helper-text invalid"
            v-if="$v.phone.$dirty && !$v.phone.required"
          >Введите номер телефона</small>
          <small
            class="helper-text invalid"
            v-else-if="$v.phone.$dirty && !$v.phone.phone"
          >Номер должен начинаться с 7</small>
          <small
            class="helper-text invalid"
            v-else-if="$v.phone.$dirty && !$v.phone.minLength && $v.phone.phone"
          >Минимум {{$v.phone.$params.minLength.min}} символа</small>
        </div>
        <div>
          <select v-model="genders">
            <option disabled value>Выберите пол</option>
            <option
              v-for="option in gender"
              v-bind:value="option.value"
              :key="option.value"
            >{{ option.text }}</option>
          </select>
          <select v-model="doctor">
            <option disabled value>Лечащий врач</option>
            <option
              v-for="doctor in doctors"
              :value="doctor.value"
              :key="doctor.value"
            >{{ doctor.text }}</option>
          </select>
        </div>
        <div style="position: relative">
          <select
            :class="{invalid: $v.groups.$dirty && !$v.groups.required}"
            v-model="groups"
            multiple
            size="3"
          >
            <option
              v-for="group in client_group"
              :value="group.value"
              :key="group.value"
            >{{ group.text }}</option>
          </select>
          <small
            style="margin-left: 13vw; bottom: -1vh"
            class="helper-text invalid"
            v-if="$v.groups.$dirty && !$v.groups.required"
          >Выберите свою группу</small>
        </div>

        <br />
        <input type="checkbox" id="sms" value="sms" v-model="checkedSMS" />
        <label for="sms">Не отправлять СМС</label>
      </div>

      <div class="form_block_right">
        <h3>Адресс проживания</h3>
        <div class="qq">
          <div class="field">
            <input class="form_input" type="number" name="index" id="index" placeholder="101000" />
            <label class="form_label" for="index">Индекс</label>
          </div>
          <div class="field">
            <input class="form_input" type="text" name="country" id="country" placeholder="Россия" />
            <label class="form_label" for="country">Страна</label>
          </div>
          <div class="field">
            <input class="form_input" type="text" name="region" id="region" placeholder="Москвская" />
            <label class="form_label" for="region">Область</label>
          </div>
          <div class="field">
            <input
              :class="{invalid: ($v.sity.$dirty && !$v.sity.required)}"
              v-model="sity"
              class="form_input"
              type="text"
              name="sity"
              id="sity"
              placeholder="Москва"
            />
            <label class="form_label" for="sity">
              Город
              <span class="starCheck">*</span>
            </label>
            <small
              class="helper-text invalid"
              v-if="$v.sity.$dirty && !$v.sity.required"
            >Введите свой город</small>
          </div>
          <div class="field">
            <input class="form_input" type="text" name="street" id="street" placeholder="Ленина" />
            <label class="form_label" for="street">Улица</label>
          </div>
          <div class="field">
            <input class="form_input" type="text" name="dwelling" id="dwelling" placeholder="12а" />
            <label class="form_label" for="dwelling">дом</label>
          </div>
        </div>
        <h3>Паспортные данные</h3>
        <div class="qq">
          <div style="position: relative" class="document">
            <select
              style="width: 100%"
              v-model="document"
              :class="{invalid: $v.document.$dirty && !$v.document.required}"
            >
              <option disabled value>Тип документа</option>
              <option
                v-for="document in documents"
                :value="document.value"
                :key="document.value"
              >{{ document.text }}</option>
            </select>
            <small
              style="margin-left: 10vw; bottom: -1vh"
              class="helper-text invalid"
              v-if="$v.document.$dirty && !$v.document.required"
            >Выберите документ</small>
          </div>
          <br />
          <div class="field">
            <input class="form_input" type="number" name="series" id="series" placeholder="0000" />
            <label class="form_label" for="series">Серия</label>
          </div>
          <div class="field">
            <input class="form_input" type="number" name="number" id="number" placeholder="000000" />
            <label class="form_label" for="number">Номер</label>
          </div>
          <div class="field">
            <input
              style="width: 78%; margin-right: 7vw"
              class="form_input"
              type="text"
              name="issued"
              id="issued"
              placeholder=" "
            />
            <label class="form_label" for="issued">Кем выдан</label>
          </div>
          <br />

          <label class="form_label data_born" for="issue">Дата выдачи:</label>
          <input
            v-model="issue"
            class="form_input data_born"
            type="date"
            id="issue"
            name="trip-start"
            value="2016-01-01"
            min="1954-01-01"
            max="2016-12-01"
            :class="{invalid: ($v.issue.$dirty && !$v.issue.required)}"
          />
          <small
            style="bottom: -0.5vh"
            class="helper-text invalid"
            v-if="$v.issue.$dirty && !$v.issue.required"
          >Выберите дату выдачи</small>
        </div>
      </div>
      <div class="btn">
        <button type="submit">Отправить</button>
      </div>
    </form>
  </div>
</template>

<script>
import { required, minLength, helpers } from "vuelidate/lib/validators";
const phone = (value) => value[0] == "7";
export default {
  validations: {
    name: {
      required,
      minLength: minLength(4),
    },
    surname: { required },
    data_born: { required },
    phone: { required, minLength: minLength(11), phone },
    groups: { required },
    document: { required },
    sity: { required },
    issue: { required },
  },
  data: () => ({
    checkedSMS: false,
    name: "",
    surname: "",
    genders: "",
    document: "",
    sity: "",
    data_born: "",
    middle_name: "",
    issue: "",

    phone: "",
    groups: [],
    doctor: "",
    gender: [
      { text: "Мужской", value: "male" },
      { text: "Женский", value: "female" },
    ],
    doctors: [
      { text: "Иванов", value: "Ivanov" },
      { text: "Захаров", value: "Zakharov" },
      { text: "Чернышева", value: "Chernysheva" },
    ],
    client_group: [
      { text: "VIP", value: "VIP" },
      { text: "Проблемные", value: "Problematic" },
      { text: "ОМС", value: "OMS" },
    ],
    documents: [
      { text: "Паспорт", value: "passport" },
      { text: "Свидетельство о рождении", value: "birth_certificate" },
      { text: "Вод. удостоверение", value: "driver's_license" },
    ],
  }),
  methods: {
    async submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }
      const formData = {
        name: this.name,
        surname: this.surname,
        middle_name: this.middle_name,
        data_born: this.data_born,
        phone: this.phone,
        genders: this.genders,
        doctor: this.doctor,
        checkedSMS: this.checkedSMS,
        client_group: this.groups[0],
      };
      alert("и тут появляется красивое уведомление сверху");
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
}

h3 {
  margin-top: 2vh;
}

input[type="number"] {
  -moz-appearance: textfield;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

  display: flex;

  max-width: 100vw;
  height: 100vh;

  color: black;
  background: rgb(128, 128, 128);

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  justify-content: center;
  align-items: center;
}

.btn {
  display: flex;

  width: 100%;

  justify-content: center;

  button {
    width: 70%;
    margin: 2vw;

    cursor: pointer;
    text-transform: uppercase;

    color: white;
    border: 0;
    border-radius: 20px;
    outline: none;
    background: black;
  }

  button:hover {
    background: rgb(71, 71, 71);
  }
}

select {
  width: 40%;
  margin: 1vh 0.2vw;

  text-align: center;

  border-radius: 10px;
  outline: none;
}

.patronymic {
  width: 100%;

  input {
    width: 80%;
  }
}

.form_block {
  display: flex;
  overflow: hidden;

  width: 70vw;
  height: 70vh;
  padding: 5px;

  text-align: center;

  border: 1px solid black;
  background: white;
  box-shadow: 1px 4px 5px 9px rgb(78, 75, 75);

  flex-wrap: wrap;

  .form_block_left,
  .form_block_right {
    position: relative;

    display: block;

    width: 49.6%;

    flex: 1 1 auto;
  }
}

.qq {
  display: flex;

  width: 100%;

  flex-wrap: wrap;
}

.field {
  display: flex;
  position: relative;
  margin-bottom: 1em;

  flex-flow: column-reverse;
}

.starCheck {
  color: red;
  font-size: 12px;
}

.form_label,
.form_input {
  transition: all 0.2s;

  touch-action: manipulation;
}

.form_input {
  font-size: 1.5em;

  position: relative;

  width: 12vw;
  min-width: 140px;
  margin-left: 4vw;
  padding: 0;

  cursor: text;

  border: 0;
  border-bottom: 1px solid #ccc;
  border-radius: 0;

  -webkit-appearance: none;
}

.form_input:focus {
  border-bottom: 1px solid #666;
  outline: 0;
}

.form_label {
  font-size: 10px;

  margin-bottom: 10px;
  margin-left: 4vw;

  text-align: left;
  letter-spacing: 0.05em;

  color: gray;
}

.form_input:placeholder-shown + .form_label {
  overflow: hidden;

  cursor: text;
  transform: translate(0, 2.125rem) scale(1.5);
  transform-origin: left bottom;
  white-space: nowrap;
  text-overflow: ellipsis;
}

::-webkit-input-placeholder {
  transition: inherit;

  opacity: 0;
}

.form_input:focus::-webkit-input-placeholder {
  opacity: 1;
}

.form_input:focus + .form_label {
  cursor: pointer;
  transform: translate(0, 0) scale(1);
}

.phone,
.data_born {
  width: 80%;
}

.document {
  width: 80%;
  height: 100%;
  margin-top: 2vh;
  margin-left: 4vw;
}

small {
  font-size: 10px;
  color: red;
  display: block;
  position: absolute;

  bottom: -2vh;
  margin-left: 6vh;
}

@media screen and (max-width: 1038px) {
  .form_block {
    height: 95%;
  }
}

@media screen and (max-width: 448px) {
  .form_block {
    position: relative;

    overflow-x: hidden;
    overflow-y: auto;

    height: 90%;

    justify-content: center;
  }

  .form_block .form_block_left,
  .form_block .form_block_right {
    width: 100%;
  }

  .form_input {
    width: 80%;
  }
}
</style>
