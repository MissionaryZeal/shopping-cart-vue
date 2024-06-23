<script>
export default {
  data() {
    return {
      cartItemDisabled: [],
    };
  },
  props: {
    carts: {
      type: Array,
    },
  },

  computed: {
    calculateCartTotal() {
      const savingPercentage = 1.5;
      const taxPercentage = 2.0;

      const originalPrice = this.carts.reduce(
        (acc, cart) => acc + cart.price * cart.qty,
        0
      );

      const savings = (originalPrice * savingPercentage) / 100;

      const tax = (originalPrice * taxPercentage) / 100;

      const finalTotal = originalPrice + tax - savings;

      return {
        originalPrice,
        savings,
        tax,
        finalTotal,
      };
    },
  },

  methods: {
    goToProduct() {
      this.$emit("go-to-product");
    },

    updateQty(product, action) {
      let cartItems = JSON.parse(localStorage.getItem("cartItems")) || [];

      const cartItemIndex = cartItems.findIndex((item) => item.id === product.id);

      if (cartItemIndex !== -1) {
        const cartItem = cartItems[cartItemIndex];

        if (action === "decrease") {
          if (product.qty > 1) {
            cartItem.qty -= 1;
          } else {
            cartItemDisabled.push(product.id);
          }
        } else {
          cartItem.qty += 1;
        }

        cartItems[cartItemIndex] = cartItem;

        localStorage.setItem("cartItems", JSON.stringify(cartItems));

        this.$emit("update-to-cart");
      }
    },

    removeFromCart(productId) {
      if (window.confirm("Are you sure you want to remove this item from the cart?")) {
        let cartItems = JSON.parse(localStorage.getItem("cartItems")) || [];

        const cartItemIndex = cartItems.findIndex((item) => item.id === productId);

        cartItems.splice(cartItemIndex, 1);

        localStorage.setItem("cartItems", JSON.stringify(cartItems));

        this.$emit("update-to-cart");
      }
    },
  },
};
</script>

<template>
  <section class="bg-white py-8 antialiased dark:bg-gray-900 md:py-16">
    <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
      <h2 class="text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">
        Shopping Cart
      </h2>

      <div class="mt-6 sm:mt-8 md:gap-6 lg:flex lg:items-start xl:gap-8" v-if="carts.length > 0">
        <div class="mx-auto w-full flex-none lg:max-w-2xl xl:max-w-4xl">
          <div class="space-y-6">
            <div
              class="rounded-lg border border-gray-200 bg-white p-4 shadow-sm dark:border-gray-700 dark:bg-gray-800 md:p-6"
              v-for="product in carts"
            >
              <div
                class="space-y-4 md:flex md:items-center md:justify-between md:gap-6 md:space-y-0"
              >
                <a href="#" class="shrink-0 md:order-1">
                  <img
                    class="hidden h-20 w-20 dark:block"
                    :src="product.image"
                    alt="imac image"
                  />
                </a>

                <label for="counter-input" class="sr-only">Choose quantity:</label>
                <div class="flex items-center justify-between md:order-3 md:justify-end">
                  <div class="flex items-center">
                    <button
                      type="button"
                      id="decrement-button"
                      data-input-counter-decrement="counter-input"
                      class="inline-flex h-5 w-5 shrink-0 items-center justify-center rounded-md border border-gray-300 bg-gray-100 hover:bg-gray-200 focus:outline-none focus:ring-2 focus:ring-gray-100 dark:border-gray-600 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700"
                      :disabled="cartItemDisabled.includes(product.id)"
                      @click="updateQty(product, 'decrease')"
                    >
                      <svg
                        class="h-2.5 w-2.5 text-gray-900 dark:text-white"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 18 2"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M1 1h16"
                        />
                      </svg>
                    </button>
                    <input
                      type="text"
                      id="counter-input"
                      data-input-counter
                      class="w-10 shrink-0 border-0 bg-transparent text-center text-sm font-medium text-gray-900 focus:outline-none focus:ring-0 dark:text-white"
                      placeholder=""
                      :value="product.qty"
                      required
                    />
                    <button
                      type="button"
                      id="increment-button"
                      data-input-counter-increment="counter-input"
                      class="inline-flex h-5 w-5 shrink-0 items-center justify-center rounded-md border border-gray-300 bg-gray-100 hover:bg-gray-200 focus:outline-none focus:ring-2 focus:ring-gray-100 dark:border-gray-600 dark:bg-gray-700 dark:hover:bg-gray-600 dark:focus:ring-gray-700"
                      @click="updateQty(product, 'increase')"
                    >
                      <svg
                        class="h-2.5 w-2.5 text-gray-900 dark:text-white"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 18 18"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M9 1v16M1 9h16"
                        />
                      </svg>
                    </button>
                  </div>
                  <div class="text-end md:order-4 md:w-32">
                    <p class="text-base font-bold text-gray-900 dark:text-white">
                      ${{ product.price }}
                    </p>
                  </div>
                </div>

                <div class="w-full min-w-0 flex-1 space-y-4 md:order-2 md:max-w-md">
                  <a
                    href="#"
                    class="text-base font-medium text-gray-900 hover:underline dark:text-white"
                    >{{ product.name }}</a
                  >

                  <div class="flex items-center gap-4">
                    <button
                      type="button"
                      class="inline-flex items-center text-sm font-medium text-gray-500 hover:text-gray-900 hover:underline dark:text-gray-400 dark:hover:text-white"
                    >
                      <svg
                        class="me-1.5 h-5 w-5"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        fill="none"
                        viewBox="0 0 24 24"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M12.01 6.001C6.5 1 1 8 5.782 13.001L12.011 20l6.23-7C23 8 17.5 1 12.01 6.002Z"
                        />
                      </svg>
                      Add to Favorites
                    </button>

                    <button
                      type="button"
                      class="inline-flex items-center text-sm font-medium text-red-600 hover:underline dark:text-red-500"
                      @click="removeFromCart(product.id)"
                    >
                      <svg
                        class="me-1.5 h-5 w-5"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        fill="none"
                        viewBox="0 0 24 24"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M6 18 17.94 6M18 18 6.06 6"
                        />
                      </svg>
                      Remove
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="mx-auto mt-6 max-w-4xl flex-1 space-y-6 lg:mt-0 lg:w-full">
          <div
            class="space-y-4 rounded-lg border border-gray-200 bg-white p-4 shadow-sm dark:border-gray-700 dark:bg-gray-800 sm:p-6"
          >
            <p class="text-xl font-semibold text-gray-900 dark:text-white">
              Order summary
            </p>

            <div class="space-y-4">
              <div class="space-y-2">
                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">
                    Original price
                  </dt>
                  <dd class="text-base font-medium text-gray-900 dark:text-white">
                    ${{ calculateCartTotal.originalPrice.toFixed(2) }}
                  </dd>
                </dl>

                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">
                    Savings
                  </dt>
                  <dd class="text-base font-medium text-green-600">
                    -${{ calculateCartTotal.savings.toFixed(2) }}
                  </dd>
                </dl>

                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">
                    Tax
                  </dt>
                  <dd class="text-base font-medium text-gray-900 dark:text-white">
                    ${{ calculateCartTotal.tax.toFixed(2) }}
                  </dd>
                </dl>
              </div>

              <dl
                class="flex items-center justify-between gap-4 border-t border-gray-200 pt-2 dark:border-gray-700"
              >
                <dt class="text-base font-bold text-gray-900 dark:text-white">Total</dt>
                <dd class="text-base font-bold text-gray-900 dark:text-white">
                  ${{ calculateCartTotal.finalTotal.toFixed(2) }}
                </dd>
              </dl>
            </div>

            <a
              href="#"
              class="flex w-full items-center justify-center rounded-lg bg-primary-700 px-5 py-2.5 text-sm font-medium text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
              >Proceed to Checkout</a
            >

            <div class="flex items-center justify-center gap-2">
              <span class="text-sm font-normal text-gray-500 dark:text-gray-400">
                or
              </span>
              <a
                href="#"
                title=""
                @click.prevent="goToProduct"
                class="inline-flex items-center gap-2 text-sm font-medium text-primary-700 underline hover:no-underline dark:text-primary-500 text-white"
              >
                Continue Shopping
                <svg
                  class="h-5 w-5"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M19 12H5m14 0-4 4m4-4-4-4"
                  />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="text-center" v-else>
         <h2 class="text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">
        Cart Empty
      </h2>
      </div>
    </div>
  </section>
</template>
