<script>
export default {
  data() {
    return {
      count: 0,
      products: [],
      commands: [],
      cuurentProcduct: null,
    };
  },
  mounted() {
    try {
      fetch("data.json")
        .then((res) => res.json())
        .then((data) => (this.products = data.products));
    } catch (e) {
      console.error(e);
    }
  },
  methods: {
    click() {
      this.count++;
      console.log(this.products);
    },
    command(product) {
      this.commands.push(product);
      product.count++;
    },
    removeCommand(command, index) {
      if (command.count > 0) {
        command.count--;
      }
      this.commands = this.commands.filter((_, command) => {
        return command !== index;
      });
    },
  },
};
</script>

<template>
  <section>
    <h2 class="choose">Choisir votre commande</h2>
    <div class="card-wrapper">
      <div
        class="card"
        v-for="(product, index) in products"
        :key="`i-${index}`"
      >
        <h2 class="product-name">{{ product.name }}</h2>
        <div class="card-body">
          <h2 class="price">{{ product.price }}</h2>
          <p class="count">{{ product.count }}</p>
        </div>
        <div class="btn-wrap">
          <button class="btn-command" @click="command(product)">
            commander
          </button>
        </div>
      </div>
    </div>
  </section>
  <section>
    <h2 class="choose" v-if="commands.length > 0">Liste des commandes</h2>
    <div class="command-wrap">
      <div
        v-for="(command, $index) in commands"
        :key="`i${$index}`"
        class="card-command"
      >
        <h2 class="command-name">{{ command.name }}</h2>
        <div class="card-body">
          <h2 class="command-price">{{ command.price }}</h2>
        </div>
        <div class="btn-wrap">
          <button class="command-btn" @click="removeCommand(command, $index)">
            remove
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.card-wrapper {
  @apply flex items-center justify-around my-5;
}
.choose {
  @apply text-center font-mono text-[50px] text-black;
}
.card {
  @apply w-[200px] text-center border-2 border-black py-5;
}

.product-name {
  @apply text-[30px] text-black font-thin;
}

.price {
  @apply text-red-600 font-bold text-[25px];
}
.count {
  @apply text-[20px] font-serif;
}
.btn-command {
  @apply w-full px-7 bg-black text-white py-3 rounded;
}
.btn-wrap {
  @apply w-[150px] mx-auto;
}
.command-wrap {
  @apply grid grid-cols-4;
}

.card-command {
  @apply w-full bg-black py-7 px-4;
}
.command-name {
  @apply text-white text-[40px] font-bold;
}
.command-price {
  @apply text-white text-center font-mono text-[30px] my-4;
}
.command-btn {
  @apply w-full bg-white text-black rounded px-5 py-3;
}
</style>
