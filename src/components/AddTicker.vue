<template>
  <section>
    <div class="flex">
      <div class="max-w-xs">
        <label for="wallet" class="block text-sm font-medium text-gray-700"
          >Тикер</label
        >
        <div class="mt-1 relative rounded-md shadow-md">
          <input
            v-model="ticker"
            type="text"
            name="wallet"
            id="wallet"
            class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
            placeholder="Например DOGE"
          />
        </div>
        <div class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap">
          <span
            v-for="(symbol, idx) in filteredCoinsSymbols"
            :key="idx"
            @click="changeTicker(symbol)"
            class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
          >
            {{ symbol }}
          </span>
        </div>
        <div v-if="checkTickers" class="text-sm text-red-600">
          Такой тикер уже добавлен
        </div>
      </div>
    </div>
    <add-button :disabled="disabled" @click="add" />
  </section>
</template>

<script>
import addButton from "@/components/AddButton";
export default {
  name: "AddTicker",
  components: {
    addButton,
  },
  props: {
    checkTickers: {
      type: Boolean,
      required: false,
      default: false,
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  data() {
    return {
      ticker: "",
      coins: [],
    };
  },
  created() {
    this.coinsRequest();
  },
  computed: {
    filteredCoinsSymbols() {
      return this.coins
        .filter((coin) => coin.Symbol.includes(this.ticker))
        .map((coin) => coin.Symbol)
        .slice(0, 4);
    },
  },

  methods: {
    add() {
      this.$emit("add-ticker", this.ticker);
      this.ticker = "";
    },
    changeTicker(t) {
      this.ticker = t;
    },
    async coinsRequest() {
      const f = await fetch(
        "https://min-api.cryptocompare.com/data/all/coinlist?summary=true"
      );
      const data = await f.json();
      this.coins = Object.values(data.Data);
    },
  },
};
</script>

<style scoped></style>
