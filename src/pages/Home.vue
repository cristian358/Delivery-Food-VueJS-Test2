<script setup>
import { ref } from 'vue'

// Первая задача 
const inputArrayFirst = ref('');
const result = ref(null);
const checkValidationSequence = ref(false)
const checkSequence = () => {
  const numbers = inputArrayFirst.value.split(',').map(Number);
  const sortedNumbers = [...numbers].sort((a, b) => a - b);
  
  const allNumbers = numbers.every(item => !isNaN(item));
  
  if (allNumbers && (inputArrayFirst.value.trim().length > 0) ) {
    checkValidationSequence.value = true
  for (let i = 0; i < sortedNumbers.length - 1; i++) {
    if (sortedNumbers[i + 1] - sortedNumbers[i] !== 1) {
      result.value = false;
      return;
    }
  }
  
  result.value = true;
  
  } else { result.value = false; checkValidationSequence.value = false }
}

// Вторая задача 
const inputArrayStrunique = ref('');
const resultSecond = ref(false);

const getUniqueSortedArray = (arr) => {
  const uniqueArray = [...new Set(arr)];
  return uniqueArray.sort((a, b) => a - b);
};

const uniqueSortedArray = ref(getUniqueSortedArray(inputArrayStrunique.value));

const updateArray = () => {
  const inputArrayunique = ref(inputArrayStrunique.value.split(',').map(Number));
  const allNumbers = inputArrayunique.value.every(item => !isNaN(item));
  
  if (allNumbers && (inputArrayStrunique.value.trim().length > 0) ) {
    resultSecond.value = true
    inputArrayunique.value = inputArrayStrunique.value.split(',').map(Number);
    uniqueSortedArray.value = getUniqueSortedArray(inputArrayunique.value);
  } else {
    resultSecond.value = false
  }
}

// Третья задача 

const inputArrayStrthird = ref('');
const resultthird = ref([]);
const resultThird = ref(false);

const countOccurrences = () => {
  
  const arr = inputArrayStrthird.value.split(',').map(Number);
  const allNumbers = arr.every(item => !isNaN(item));
  
  if (allNumbers && inputArrayStrthird.value.trim().length > 0 ) {
    resultThird.value = true
    const occurrenceMap = {};
    
    arr.forEach(num => {
      occurrenceMap[num] = (occurrenceMap[num] || 0) + 1;
    });
    
    resultthird.value = Object.keys(occurrenceMap).map(key => {
      return { [key]: occurrenceMap[key] };
    });
  } else {
    resultThird.value = false
  }
}
</script>

<template>
  <div class="center container">
   <h2 style="text-align: center;"> Part 2 </h2>
   <!-- // Первая задача -->
   <div>
   <p>1. Написать функцию которая: учитывает массив несортированных чисел и определяет являются ли числа в массиве последовательными, например :
    <br><br> a. Если массив равен [5, 2, 3, 1, 4] тогда функция должна вернуть true потому что массив содержит последовательные числа от 1 до 5 (1,2,3,4,5)
    <br> b. Если массив равен [34, 23, 52, 12, 3] тогда функция должна возвращать значения  false потому что элементы не являются последовательными.
    <br>с. Если массив равен [7, 6, 5, 5, 3, 4] тогда функция должна возвращать значения false, потому что 5  и 5 не являются последовательными </p>
    <input type="text" class="input inputSize" v-model="inputArrayFirst" placeholder="5, 2, 3, 1, 4">
    <button @click="checkSequence" class="button button-primary inline">Проверить последовательность</button>
    <div v-if="result !== null && checkValidationSequence" class="response">
      {{ result ? 'Числа в массиве последовательны (true)' : 'Элементы не являются последовательными (false)' }}
    </div>
    <div v-if="!checkValidationSequence" class="response">
      <p class="incorrect">Неправильный ввод массива (Пример : 1, 2, 3 ; ,,,,,). Input должен содержить только числа</p>
    </div>
  </div>
  <!-- // Вторая задача  -->
  <div>
    <p>2. Необходимо реализовать функцию, которая принимает этот массив в качестве аргумента и возвращает новый массив, содержащий только уникальные элементы. Новый массив должен быть отсортирован по возрастанию.
      <br><br>Примеры:
      <br>Входной массив: [1, 3, 2, 2, 4, 3, 5, 6, 5] Выходной массив: [1, 2, 3, 4, 5, 6]
      <br>Входной массив: [9, 9, 9, 9, 9] Выходной массив: [9]
      <br>Входной массив: [1, 2, 3, 4, 5] Выходной массив: [1, 2, 3, 4, 5] 
    </p>
      <div>
        <input type="text" v-model="inputArrayStrunique" class="input inputSize" placeholder="1, 3, 2, 2, 4, 3, 5, 6, 5">
        <button @click="updateArray" class="button button-primary inline">Обновить массив</button>
        <div class="response">
          <p v-if="resultSecond">Уникальные элементы, отсортированные по возрастанию: {{ uniqueSortedArray }}</p>
          <p v-else class="incorrect">Неправильный ввод массива (Пример : 1, 2, 3 ; ,,,,,). Input должен содержить только числа</p>
        </div>
    </div>
  </div>
<!-- // Третья задача -->
  <div>
   <p>3. Написать функцию которая принимает в качестве аргумента массив чисел и возвращает новый массив с количеством повторений первоначального массива {число: кол во повторений}
    <br><br>Примеры:
    <br>Входной массив: [1, 3, 2, 2, 4, 3, 5, 6, 5] Выходной массив: [{1:1}, {3: 2}, {2:2}, {4:1},{5:2},{6:1}]
    </p>
    <div>
      <input type="text" v-model="inputArrayStrthird" class="input inputSize" placeholder="1, 3, 2, 2, 4, 3, 5, 6, 5">
      <button @click="countOccurrences" class="button button-primary inline">Подсчитать повторения</button>
      <div v-if="resultThird == true" class="response">
        <p>Результат подсчета повторений:</p>
        <ul>
          <li v-for="(item, index) in resultthird" :key="index">{{ item }}</li>
        </ul>
      </div>
      <div v-else class="incorrect"> <p>Неправильный ввод массива (Пример : 1, 2, 3  ; ,,,,,). Input должен содержить только числа</p></div>
    </div>
  </div>  
</div>
</template>
