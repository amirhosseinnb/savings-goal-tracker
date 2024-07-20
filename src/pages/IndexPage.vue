<template>
  <q-page padding class="bg-dark text-white">
    <div class="neumorphic-form">
      <q-form class="form">
        <div class="neumorphic-input">
          <q-input
            v-model="formattedGoalAmount"
            label="مبلغ هدف برای خرید (به تومان)"
            type="text"
            @input="updateGoalAmount"
            outlined
          />
        </div>
        <div class="neumorphic-input">
          <q-input
            v-model="formattedDailyAmount"
            label="مقدار پول روزانه برای پس‌انداز (به تومان)"
            type="text"
            @input="updateDailyAmount"
            outlined
          />
        </div>
        <div class="neumorphic-input">
          <q-input
            v-model="formattedTotalDays"
            label="تعداد روزهایی که می‌خواهید پس‌انداز کنید"
            type="text"
            @input="updateTotalDays"
            outlined
          />
        </div>
      </q-form>
      <div v-if="totalSavings !== null" class="text-center">
        <p class="text-h6">
          شما پس از {{ totalDays }} روز مبلغ {{ formattedTotalSavings }} تومان
          پس‌انداز خواهید داشت
        </p>
        <p v-if="totalSavings >= goalAmount" class="text-h6 text-green-13">
          تبریک! شما به هدف خود برای خرید رسیده‌اید
        </p>
        <p v-else class="text-h6 text-red-13">
          شما هنوز به هدف خود نرسیده‌اید. شما
          {{ formattedRemainingAmount }} تومان دیگر نیاز دارید
        </p>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const dailyAmount = ref(0);
const totalDays = ref(0);
const goalAmount = ref(0);
const totalSavings = ref(null);

const calculateSavings = () => {
  totalSavings.value = dailyAmount.value * totalDays.value;
};

// Initial calculation
calculateSavings();

const formatNumber = (num) => {
  return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
};

const parseNumber = (str) => {
  return parseFloat(str.replace(/,/g, "")) || 0;
};

// Formatted inputs
const formattedDailyAmount = computed({
  get() {
    return formatNumber(dailyAmount.value);
  },
  set(value) {
    dailyAmount.value = parseNumber(value);
  },
});

const formattedTotalDays = computed({
  get() {
    return formatNumber(totalDays.value);
  },
  set(value) {
    totalDays.value = parseNumber(value);
  },
});

const formattedGoalAmount = computed({
  get() {
    return formatNumber(goalAmount.value);
  },
  set(value) {
    goalAmount.value = parseNumber(value);
  },
});

// Watchers to recalculate savings
watch([dailyAmount, totalDays], calculateSavings);

// Formatted total savings and remaining amount
const formattedTotalSavings = computed(() => {
  return totalSavings.value !== null ? formatNumber(totalSavings.value) : "";
});

const formattedRemainingAmount = computed(() => {
  if (totalSavings.value !== null && totalSavings.value < goalAmount.value) {
    return formatNumber(goalAmount.value - totalSavings.value);
  }
  return "";
});

// Update functions for inputs
const updateDailyAmount = (event) => {
  formattedDailyAmount.value = event.target.value;
};

const updateTotalDays = (event) => {
  formattedTotalDays.value = event.target.value;
};

const updateGoalAmount = (event) => {
  formattedGoalAmount.value = event.target.value;
};
</script>

<style scoped>
.q-page {
  max-width: 600px;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.bg-dark {
  background-color: #121212;
}

.neumorphic-form {
  background: #1e1e1e;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 9px 9px 16px #0d0d0d, -9px -9px 16px #2f2f2f;
  width: 100%;
}

.neumorphic-input {
  margin-bottom: 20px;
  box-shadow: inset 4px 4px 8px #0d0d0d, inset -4px -4px 8px #2f2f2f;
  border-radius: 10px;
  overflow: hidden;
}

.neumorphic-btn {
  display: flex;
  justify-content: center;
}

.neumorphic-btn-inner {
  background: #1e1e1e;
  border-radius: 12px;
  box-shadow: 4px 4px 8px #0d0d0d, -4px -4px 8px #2f2f2f;
  padding: 10px 20px;
}

.text-white {
  color: white;
}
</style>
