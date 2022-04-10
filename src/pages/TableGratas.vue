<template>
  <div class="q-pa-md">
    <q-markup-table>
      <thead>
        <tr id="titles">
          <th
            :class="{ 'text-left': index === 0, 'text-right': index > 0 }"
            v-for="(title, index) in titles"
            :key="title"
          >
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="user in users"
          :key="user.id"
          class="rows"
          :class="{
            clicked: user.isClicked,
            'wrong-entry': user.wrongEntry,
            late: user.late,
            'on-time': user.onTime,
          }"
          @click="click(user.id)"
        >
          <td class="text-left">{{ user.name }}</td>
          <td class="text-right">{{ user.bookname }}</td>
          <td class="text-right">{{ user.writer }}</td>
          <td class="text-right">{{ user.borrowedDate }}</td>
          <td class="text-right">{{ user.deliveryDate }}</td>
          <td class="text-right">{{ user.returnedDate }}</td>
        </tr>
      </tbody>
    </q-markup-table>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
export default {
  setup() {
    const titles = [
      "Fullname",
      "Bookname",
      "Writer",
      "Borrowed Date",
      "Delivery Date",
      "Returned Date",
    ];
    const users = ref([
      {
        id: 1,
        name: "Nevin GEMICI",
        bookname: "Murder In Vein",
        writer: "Sue Ann JAFFARIAN",
        borrowedDate: "2021-1-23",
        deliveryDate: "2021-7-13",
        returnedDate: "2020-5-12",
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
      {
        id: 2,
        name: "Mert OMGEN",
        bookname: "The Lord Of The Rings: The Fellowship of the Ring",
        writer: "J.R.R. TOLKIEN",
        borrowedDate: "2020-8-17",
        deliveryDate: "2020-10-15",
        returnedDate: "2020-12-12",
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
      {
        id: 3,
        name: "Alpcan ARSLAN",
        bookname: "Alex Ferguson: My Autobiography",
        writer: "Sir Alex FERGUSON",
        borrowedDate: "2015-1-12",
        deliveryDate: "2015-12-20",
        returnedDate: "2021-10-12",
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
      {
        id: 4,
        name: "Mert TOK",
        bookname: "Is God a Mathematician",
        writer: "Maria LIVIO",
        borrowedDate: "2018-3-7",
        deliveryDate: "2018-8-7",
        returnedDate: null,
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
      {
        id: 5,
        name: "Salih SENGONUL",
        bookname: "The Marriage Pact",
        writer: "Michelle RICHMOND",
        borrowedDate: "2020-8-17",
        deliveryDate: "2020-10-15",
        returnedDate: "2020-9-12",
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
      {
        id: 6,
        name: "Batuhan KARAHAN",
        bookname: "The Einstein Enigma",
        writer: "Jose Rodrigues dos SANTOS",
        borrowedDate: "2020-3-22",
        deliveryDate: "2020-12-30",
        returnedDate: null,
        isClicked: false,
        wrongEntry: false,
        late: false,
        onTime: false,
      },
    ]);

    function click(userId) {
      const foundedUser = users.value.find((u) => u.id === userId);
      foundedUser.isClicked = !foundedUser.isClicked;
    }

    onBeforeMount(() => {
      if (users.value && users.value.length > 0) {
        for (let i = 0; i < users.value.length; i++) {
          if (
            users.value[i].borrowedDate !== null &&
            users.value[i].deliveryDate !== null &&
            users.value[i].returnedDate !== null
          ) {
            if (
              !compareTimes(
                users.value[i].borrowedDate,
                users.value[i].returnedDate
              )
            )
              users.value[i].wrongEntry = true;
            if (
              compareTimes(
                users.value[i].deliveryDate,
                users.value[i].returnedDate
              )
            )
              users.value[i].late = true;
            if (
              !compareTimes(
                users.value[i].deliveryDate,
                users.value[i].returnedDate
              ) &&
              users.value[i].wrongEntry === false
            )
              users.value[i].onTime = true;
          }
        }
      } else alert("There are no users!");
    });

    function compareTimes(givenTime1, givenTime2) {
      let time1 = givenTime1.split("-").map((elem) => parseInt(elem, 10));
      let time2 = givenTime2.split("-").map((elem) => parseInt(elem, 10));
      if (time2[0] > time1[0]) return true;
      else if (time2[0] < time1[0]) return false;
      else if (time2[1] > time1[1]) return true;
      else if (time2[1] < time1[1]) return false;
      else if (time2[2] > time1[2]) return true;
      else if (time2[2] === time1[2]) return true;
      else return false;
    }

    return {
      users,
      click,
      titles,
    };
  },
};
</script>

<style scoped>
#titles {
  background-color: turquoise;
  color: white;
}
.rows :hover {
  cursor: pointer;
}
.wrong-entry {
  background-color: purple;
}
.late {
  background-color: red;
}
.on-time {
  background-color: green;
}
.clicked {
  color: orange;
  font-weight: bold;
}
</style>
