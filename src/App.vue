<script setup>
  import { ref, watch, computed } from 'vue';
  import Header from './components/Header.vue';
  import Button from "./components/Button.vue";
  import { calcularTotalPagar } from "./helpers";


  const cantidad = ref(10000);
  const meses = ref(6);
  const total = ref(0);
  const MIN = 0;
  const MAX = 20000;
  const STEP = 100;

  const moneyFormater = (valor) => {
    const formatter = new Intl.NumberFormat('en-US', {
      style: "currency",
      currency: "USD",
    });
    return formatter.format(valor);
  };

  watch([cantidad, meses], () => {
    total.value = calcularTotalPagar(cantidad.value, meses.value);
  }, { inmediate: true });

  const pagoMensual = computed(()=> {
    return total.value / meses.value;
  });

  const handleClickDecrement = () => {
    const value = cantidad.value - STEP;

    if(value < MIN) {
      return;
    }

    cantidad.value = value;
  }

  const handleClickIncrement = () => {
    const value = cantidad.value + STEP;

    if(value > MAX) {
      return;
    }

    cantidad.value = value;
  }

</script>

<template>
  <section class="my-20 max-w-lg mx-auto bg-white rounded-lg shadow p-10">
    <article>
      <Header />

      <div class="flex justify-between mt-5">
        <Button
          :operator="'-'"
          @fn="handleClickDecrement"
        />
        <Button
          :operator="'+'"
          @fn="handleClickIncrement"
        />
      </div>

      <div class="my-10">
        <input
          type="range"
          class="w-full bg-gray-200 accent-sky-500 hover:accent-sky-600"
          :min="MIN"
          :max="MAX"
          :step="STEP"
          v-model.number="cantidad"
        />

        <p class="text-center my-10 text-5xl font-extrabold text-sky-600">
          {{ moneyFormater(cantidad) }}
        </p>

        <h2 class="text-2xl font-extrabold text-gray-500 text-center">
          Elige un <span class="text-sky-500">plazo</span> a pagar
        </h2>

        <select
          class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500 outline-none mt-5"
          :value="meses"
          v-model.number="meses"
        >
          <option value="6">6 meses</option>
          <option value="12">12 meses</option>
          <option value="24">24 meses</option>
        </select>
      </div>

      <div v-if="total > 0" class="mt-5 space-y-3 bg-gray-200 p-5 rounded-lg">
        <h2 class="text-2xl font-extrabold text-gray-500 text-center">
          Resumen <span class="text-sky-500">de pagos</span>
        </h2>

        <p class="text-xl text-gray-500 text-center font-bold">
          <span class="text-sky-500">{{ meses }}</span> Meses
        </p>
        <p class="text-xl text-gray-500 text-center font-bold">
          <span class="text-sky-500">{{ moneyFormater(total) }}</span> Total a pagar
        </p>
        <p class="text-xl text-gray-500 text-center font-bold">
          <span class="text-sky-500">{{ moneyFormater(pagoMensual) }}</span> Mensuales
        </p>
      </div>

      <p v-else class="text-center font-bold text-xl text-gray-500">Añade una <span class="text-sky-500">cantidad</span> y un <span class="text-sky-500">plazo</span> a pagar</p>
    </article>
  </section>
</template>