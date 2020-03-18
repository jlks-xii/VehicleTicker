<template>
  <div>
    <div class="tickers-holder">
      <div class="tickers">
        <div class="ticker">
          <div class="count">{{ this.cars }}</div>
          <div class="vehicle-type">Cars</div>
          <div class="button reset-button" @click="resetCount('cars')">Reset</div>
        </div>
        <div class="ticker">
          <div class="count">{{ this.buses }}</div>
          <div class="vehicle-type">Buses</div>
          <div class="button reset-button" @click="resetCount('buses')">Reset</div>
        </div>
        <div class="ticker">
          <div class="count">{{ this.trucks }}</div>
          <div class="vehicle-type">Trucks</div>
          <div class="button reset-button" @click="resetCount('trucks')">Reset</div>
        </div>
      </div>
      <div class="global-actions">
        <input
          class="instance-name-input"
          type="text"
          placeholder="Instance Name"
          v-model="instance_name"
          @focus="isFocused = true"
          @blur="isFocused = false"
        >
        <div class="button save-instance" @click="saveInstance()">Save Instance</div>
        <div class="button reset-button all" @click="resetCount()">Reset All</div>
      </div>
    </div>
    <div class="instances">
      <div class="instance" v-for="(instance, index) in this.instances" :key="index">
        <div class="name">{{instance.name}}</div>
        <div class="date">{{time(instance.date)}}</div>
        <div class="data">
          <div><strong>Cars:</strong> {{instance.data.cars}}</div>
          <div><strong>Buses:</strong> {{instance.data.buses}}</div>
          <div><strong>Trucks:</strong> {{instance.data.trucks}}</div>
        </div>
        <div class="remove-action" @click="deleteInstance(index)">x</div>
      </div>
    </div>
  </div>
</template>

<script>
  import moment from 'moment';

  export default {
    data() {
      return {
        instance_name: '',
        cars: 0,
        buses: 0,
        trucks:0,
        instances: [],
        isFocused: false,
      }
    },
    mounted() {
      let instanceArr = localStorage.getItem('instanceArr');
      if(instanceArr !== null && instanceArr !== undefined) {
        this.instances = JSON.parse(instanceArr);
      } 
      this.listen();
    },
    methods: {
      listen() {
        document.addEventListener('keyup', (e) => {
          console.log(e.code);
          if(this.isFocused) {
            return;
          }

          if(e.code === 'KeyJ') {
            this.cars++;
          } else if (e.code === 'KeyK') {
            this.buses++;
          } else if (e.code === 'KeyL') {
            this.trucks++;
          }
        });
      },
      saveInstance() {
        let cars = this.cars;
        let buses = this.buses;
        let trucks = this.trucks;

        if(this.cars === 0 && this.buses === 0 && this.trucks === 0) {
          return;
        }

        const instanceData = {
          date: new Date().toISOString(),
          data: {
            cars,
            buses,
            trucks
          }
        };

        let instanceArr = localStorage.getItem('instanceArr');
        if(instanceArr !== null && instanceArr !== undefined) {
          instanceArr = JSON.parse(instanceArr);
          instanceData.name = this.instance_name ? this.instance_name : `Instance ${instanceArr.length + 1}`;
          instanceArr.push(instanceData);
        } else {
          instanceArr = [];
          instanceData.name = this.instance_name ? this.instance_name : `Instance 1`;
          instanceArr.push(instanceData);
        }

        this.instances = instanceArr;
        localStorage.setItem('instanceArr', JSON.stringify(instanceArr));
      },
      resetCount(vehicleType = '') {
        if(vehicleType === 'cars') {
          this.cars = 0;
        } else if (vehicleType === 'buses') {
          this.buses = 0;
        } else if (vehicleType === 'trucks') {
          this.trucks = 0;
        } else {
          this.cars = 0;
          this.buses = 0;
          this.trucks = 0;
          this.instance_name = '';
        }
      },
      deleteInstance(index) {
        console.log(index);
        this.instances.splice(index,1);
        localStorage.setItem('instanceArr', JSON.stringify(this.instances));
      },
      time(time) {
        return moment(time).format('Do MMM YYYY, h:mm:ss A');
      }
    },
    computed: {
    }
  }
</script>

<style lang="css" scoped>
  .tickers-holder {
    width: 100vw;
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 1fr max-content;
    align-items: center;
    justify-items: center;
    margin-top: 32px;
  }
  .tickers {
    display: grid;
    width: 50%;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1fr;
  }
  .ticker {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .count {
    font-size: 30px;
  }
  .vehicle-type {
    font-size: 28px;
    font-weight: bold;
  }
  .reset-button {
    width: 80%;
    margin-top: 12px;
    padding: 8px;
    color: hsla(268, 90%, 46%, 0.75);
    background-color: transparent;
    border-radius: 12px;
    box-shadow: 0 0 2px 0 hsla(268, 90%, 46%, 0.75), 0 2px 8px -2px rgba(0,0,0,0.15);
  }
  .global-actions {
    grid-row-start: 2;
    grid-column-start: 1;
    display: grid;
    gap: 30px;
    align-items: center;
    justify-items: center;
    grid-template-columns: repeat(3, max-content);
    margin: 32px 0;
  }
  .global-actions > * {
    min-width: 10vmax;
  }
  .reset-button.all, .save-instance {
    color: hsla(268, 90%, 46%, 0.75);
    background-color: transparent;
    border-radius: 12px;
    box-shadow: 0 0 2px 0 hsla(268, 90%, 46%, 0.75), 0 2px 8px -2px rgba(0,0,0,0.15);
    padding: 12px;
    margin-top: 0;
  }
  .button:hover {
    cursor: pointer;
    background-color: hsla(268, 90%, 46%, 0.75);
    color: #ffffff;
    box-shadow: none;
  }
  .instance-name-input {
    padding: 8px;
    font-size: 16px;
  }
  .instances {
    text-align: center;
  }
  .instance {
    display: grid;
    grid-template-columns: max-content max-content max-content min-content;
    grid-template-rows: 1fr;
    justify-content: center;
    margin: 0 auto;
    padding: 8px 0;
    box-sizing: border-box;
  }
  .name {
    font-weight: 700;
    margin-right: 8px;
  }
  .name:after {
    margin-left: 8px;
    content: '-';
  }
  .date {
    font-weight: 700;
    margin-right: 8px;
  }
  .date:after {
    margin-left: 8px;
    content: '-';
  }
  .data {
    display: flex;
    justify-content: center;
  }
  .data > div {
    margin: 0 4px;
  }
  .remove-action {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: 8px;
    width: 16px;
    height: 16px;
    border: 1px solid red;
    border-radius: 32px;
    color: red;
  }
  .remove-action:hover {
    cursor: pointer;
    background-color: red;
    color: #fff;
  }

  @media only screen and (max-width: 768px) {
    .tickers {
      width: 90%;
    }
    .global-actions {
      grid-template-columns: repeat(2, max-content);
    }
    .reset-button.all {
      grid-column-start: 1;
      grid-column-end: 3;
    }
    .instances {
      width: 100vw;
      margin: 0 auto;
    }
    .instance {
      padding: 8px 20px;
    }
  }
   @media only screen and (max-width: 540px) {
    .tickers {
      grid-template-columns: 1fr 1fr;
    }
    .ticker {
      margin-bottom: 24px;
    }
    .global-actions {
      gap: 24px;
    }
    .instances {
      margin-bottom: 20px;
    }
    .instance {
      grid-template-columns: 1fr 50px;
      grid-template-rows: repeat(3, 1fr);
      width: 100%;
    }
    .instance > * {
      margin-bottom: 4px;
    }
    .date {
      grid-row-start: 2;
    }
    .data {
      grid-row-start: 3;
    }
    .remove-action {
      grid-row-start: 1;
      grid-row-end: 4;
      grid-column-start: 2;
      grid-column-end: 3;
      justify-self: self-start;
      align-self: center;
      width: 24px;
      height: 24px;
    }
  }
</style>