<template>
<div class="container">
    <label>输入一个十进制数：</label>
    <el-input-number v-model="decimalNumber" @input="convert" placeholder="十进制数"></el-input-number>

    <label>转换为：</label>
    <el-select v-model="selectedBase" @change="convert">
      <el-option value="2">二进制</el-option>
      <el-option value="8">八进制</el-option>
      <el-option value="16">十六进制</el-option>
    </el-select>

    <p>转换结果：{{ convertedNumber }}</p>

    <h1>时钟</h1>
    <div class="clock">
      {{ currentTime }}
    </div>

    <h1>提醒事项</h1>

    <div class="add-task-form">
      <el-input v-model="newTask" type="text" placeholder="输入新的提醒事项"></el-input>
      <el-button @click="addTask">添加</el-button>
    </div>
    <ul>
      <li v-for="task in tasks" :key="task.id" :class="{ completed: task.completed }">
        <span>{{ task.title }}</span>
        <el-button @click="completeTask(task.id)" type="danger" size="mini">完成</el-button>
      </li>
    </ul>

    <h1>计算器</h1>
    <div class="calculator">
        <div class="calculator-display">
        <el-input v-model="calculation" readonly></el-input>
        </div>
    <div class="calculator-buttons">
      <el-button-group>
        <el-button v-for="button in buttons" :key="button.value" @click="handleButtonClick">
          {{ button.label }}
        </el-button>
        <el-button @click="handleDeleteButtonClick">
          删除
        </el-button>
      </el-button-group>
    </div>
  </div>
</div>
</template>


<script>
export default {
  data() {
        return {
        currentTime: '',
        decimalNumber: null,
        selectedBase: '2',
        convertedNumber: '',
        newTask: '',
        tasks: [],
        calculation: '',
        };
    },
    computed: {
        buttons() {
            return [
                { label: '7', value: '7' },
                { label: '8', value: '8' },
                { label: '9', value: '9' },
                { label: '+', value: '+' },
                { label: '4', value: '4' },
                { label: '5', value: '5' },
                { label: '6', value: '6' },
                { label: '-', value: '-' },
                { label: '1', value: '1' },
                { label: '2', value: '2' },
                { label: '3', value: '3' },
                { label: '*', value: '*' },
                { label: '0', value: '0' },
                { label: '.', value: '.' },
                { label: '=', value: '=' },
                { label: '/', value: '/' }
            ];
        }
    },
  mounted() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
  },
  methods: {
    convert() {
        if (!this.decimalNumber || isNaN(this.decimalNumber)) {
            this.convertedNumber = '';
            return;
        }
        const base = parseInt(this.selectedBase);
        this.convertedNumber = this.decimalNumber.toString(base);
    },
    updateTime() {
        const now = new Date();
        const hours = String(now.getHours()).padStart(2, '0');
        const minutes = String(now.getMinutes()).padStart(2, '0');
        const seconds = String(now.getSeconds()).padStart(2, '0');
        this.currentTime = `${hours}:${minutes}:${seconds}`;
    },
    addTask() {
        if (this.newTask) 
        {
            const newTask = {
                id: Date.now(),
                title: this.newTask,
                completed: false,
            };
            this.tasks.push(newTask);
            this.newTask = '';
        }
    },
    completeTask(taskId) {
        const task = this.tasks.find((t) => t.id === taskId);
            if (task) {
                task.completed = true;
            }
    },
    handleButtonClick(event) {
        const value = event.target.innerText;
        if (value === '=') {
            this.calculate();
        } else {
            this.calculation += value;
        }
    },
    handleDeleteButtonClick() {
      this.calculation = this.calculation.slice(0, -1);
    },
    calculate() {
        try {
            this.calculation = eval(this.calculation);
        } catch (error) {
            this.calculation = 'Error';
        }
    },
}
};
</script>
<style>
.container {
  width: 80%;
  margin: 0 auto;
}

.container label {
  margin-right: 10px;
}

.container p {
  margin-top: 10px;
}

.container h1 {
  margin-top: 20px;
}

.container .clock {
  font-size: 24px;
  margin-top: 10px;
}

.container .add-task-form {
  margin-top: 20px;
  display: flex;
  align-items: center;
}

.container .add-task-form input {
  flex: 1;
  margin-right: 10px;
}

.container ul {
  list-style-type: none;
  margin-top: 10px;
  padding-left: 0;
}

.container li {
  margin-top: 5px;
}

.container .completed {
  text-decoration: line-through;
}

.calculator {
  width: 200px;
  margin: 0 auto;
}

.calculator-display {
  margin-bottom: 10px;
}

.calculator-buttons {
  display: flex;
  justify-content: center;
}

.el-button-group {
  flex-wrap: wrap;
}

.el-button {
  width: 45px;
  height: 40px;
  margin: 5px;
}
</style>