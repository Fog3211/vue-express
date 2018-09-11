<template>
    <div class="cartcontrol">
        <transition name="move">
            <i class="cart-decrease icon-remove_circle_outline" @click="decreaseCart" v-show="food.count"></i>
        </transition>
        <div class="cart-count" v-show="food.count">{{food.count}}</div>
        <i class="cart-add icon-add_circle" @click="increaseCart"></i>
    </div>
</template>

<script>
import Vue from "vue";
export default {
    props: {
        food: {
            type: Object,
            default() {
                return {};
            }
        }
    },
    methods: {
        decreaseCart() {
            this.food.count--;
        },
        increaseCart() {
            if (!this.food.count) {
                Vue.set(this.food, "count", 1);
            } else {
                this.food.count++;
            }
        }
    }
};
</script>

<style scope>
.cartcontrol {
    font-size: 0;
}

.cartcontrol .cart-decrease {
    display: inline-block;
    width: 26px;
    height: 26px;
    font-size: 26px;
    color: #b4b4b4;
}

.cartcontrol .cart-count {
    display: inline-block;
    width: 25px;
    text-align: center;
    font-size: 14px;
    line-height: 26px;
    vertical-align: top;
}

.cartcontrol .cart-add {
    display: inline-block;
    font-size: 26px;
    color: #e6c980;
    position: relative;
    border-radius: 50%;
    text-align: center;
    vertical-align: center;
}

.move-enter-active,
.move-leave-active {
    transition: all 0.3s linear;
}
.move-enter,
.move-leave-to {
    transform: translateX(20px) rotate(180deg);
}
</style>
