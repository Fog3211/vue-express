<template>
    <div class="goods">
        <!-- 分类列表 -->
        <div class="menu-wrapper" ref="menuScroll">
            <ul>
                <li class="menu-item" :class="{'current':currentIndex === index }" v-for="(item,index) in goods" :key="index" @click="selectMenu(index)">
                    <p class="text">
                        <img class="icon" :src="item.icon" v-if="item.icon"> {{item.name}}
                    </p>
                    <i class="num" v-show="calculateCount(item.spus)">
                        {{calculateCount(item.spus)}}
                    </i>
                </li>
            </ul>
        </div>
        <!-- 商品列表 -->
        <div class="foods-wrapper" ref="foodScroll">
            <ul>
                <!-- 具体分类 -->
                <li v-for="(item,index) in goods" :key="index" class="food-list food-list-hook">
                    <h3 class="title">{{item.name}}</h3>
                    <!-- 具体的商品列表 -->
                    <ul>
                        <li v-for="(food,index) in item.spus" :key="index" class="food-item">
                            <div class="icon" :style="head_bg(food.picture)"></div>
                            <div class="content">
                                <h3 class="name">{{food.name}}</h3>
                                <p class="desc" v-if="food.description">{{food.description}}</p>
                                <div class="extra">
                                    <span class="saled">{{food.month_saled_content}}</span>
                                    <span class="praise">{{food.praise_content}}</span>
                                </div>
                                <p class="price">
                                    <span class="text">${{food.min_price}}</span>
                                    <span class="unit">/{{food.unit}}</span>
                                </p>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <app-cart-control :food="food"></app-cart-control>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <!-- 购物车 -->
        <app-shopcart :poiInfo="poiInfo" :selectFoods="selectFoods"></app-shopcart>
    </div>
</template>

<script>
import axios from "axios";
import BScroll from "better-scroll";
import ShopCart from "../shopcart/Shopcart";
import CartControl from "../cartcontrol/CartControl";

export default {
    data() {
        return {
            container: {},
            goods: [],
            listHeight: [],
            menuScroll: {},
            foodScroll: {},
            scrollY: 0,
            poiInfo: {}
        };
    },
    components: {
        "app-shopcart": ShopCart,
        "app-cart-control": CartControl
    },
    created() {
        // axios
        axios({
            url: "/api/goods",
            method: "get"
        })
            .then(response => {
                if (response.status == 200) {
                    this.container =
                        response.data.data.container_operation_source;
                    this.goods = response.data.data.food_spu_tags;
                    this.poiInfo = response.data.data.poi_info;

                    this.$nextTick(() => {
                        this.initScroll();
                        this.calculateHeight();
                    });
                }
            })
            .catch(error => {
                console.log(error);
            });
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                // 获取商品区间的范围
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                // 是否在上述区间中
                if (
                    !height2 ||
                    (this.scrollY >= height1 && this.scrollY < height2)
                ) {
                    // console.log(i)
                    return i;
                }
            }
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach(myfoods => {
                myfoods.spus.forEach(food => {
                    if (food.count > 0) {
                        foods.push(food);
                    }
                });
            });
            return foods;
        }
    },
    methods: {
        head_bg(imgName) {
            return "background-image: url(" + imgName + ");";
        },
        initScroll() {
            this.menuScroll = new BScroll(this.$refs.menuScroll, {
                click: true
            });
            this.foodScroll = new BScroll(this.$refs.foodScroll, {
                probeType: 3,
                click: true
            });
            this.foodScroll.on("scroll", pos => {
                // console.log(Math.abs(Math.round(pos.y)));
                this.scrollY = Math.abs(Math.round(pos.y));
            });
        },
        calculateHeight() {
            // 获取元素
            let foodlist = document.getElementsByClassName("food-list-hook");
            // console.log(foodlist.length);
            let height = 0;
            this.listHeight.push(height);

            for (let i = 0; i < foodlist.length; i++) {
                height += foodlist[i].clientHeight;
                this.listHeight.push(height);
            }
            // console.log(this.listHeight)
        },
        selectMenu(index) {
            // console.log(index)
            let foodlist = this.$refs.foodScroll.getElementsByClassName(
                "food-list-hook"
            );
            let element = foodlist[index];
            // console.log(element)
            // 滚动到对应元素的位置
            this.foodScroll.scrollToElement(element, 250);
        },
        calculateCount(spus) {
            let count = 0;
            spus.forEach(food => {
                if (food.count > 0) {
                    count += food.count;
                }
            });
            return count;
        }
    }
};
</script>


<style scoped>
.goods {
    display: flex;
    position: absolute;
    top: 200px;
    bottom: 51px;
    overflow: hidden;
    width: 100%;
}

.goods .menu-wrapper {
    flex: 0 0 85px;
    background: #f4f4f4;
}

.goods .foods-wrapper {
    flex: 1;
}

/* Menu item */
.goods .menu-wrapper .menu-item {
    padding: 16px 2px 15px 10px;
    border-bottom: 1px solid #e4e4e4;
    position: relative;
}

.goods .menu-wrapper .menu-item .text {
    font-size: 13px;
    color: #333333;
    line-height: 17px;
    vertical-align: middle;
}

.goods .menu-wrapper .menu-item .text .icon {
    width: 15px;
    height: 15px;
    vertical-align: middle;
}

/* 具体分类商品布局 */
.goods .foods-wrapper .food-list {
    padding: 11px;
}

.goods .foods-wrapper .food-list .food-item {
    display: flex;
    margin-bottom: 25px;
    position: relative;
}

.goods .foods-wrapper .food-list .food-item .icon {
    flex: 0 0 63px;
    background-position: center;
    background-size: 120% 100%;
    background-repeat: no-repeat;
    margin-right: 11px;
    height: 75px;
}
.goods .foods-wrapper .food-list .food-item .content {
    flex: 1;
}

/* 具体内容样式 */
.goods .foods-wrapper .food-list .food-item .content .name {
    font-size: 16px;
    line-height: 21px;
    color: #333333;
    margin-bottom: 10px;
    padding-right: 27px;
}

.goods .foods-wrapper .food-list .food-item .content .desc {
    font-size: 10px;
    line-height: 19px;
    color: #bfbfbf;
    margin-bottom: 8px;

    /* 超出部分显示省略号*/
    -webkit-line-clamp: 1;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.goods .foods-wrapper .food-list .food-item .content .extra {
    font-size: 10px;
    color: #bfbfbf;
    margin-bottom: 7px;
}
.goods .foods-wrapper .food-list .food-item .content .extra .saled {
    margin-right: 14px;
}
.goods .foods-wrapper .food-list .food-item .content .price {
    padding-top: 3px;
    font-size: 0;
}
.goods .foods-wrapper .food-list .food-item .content .price .text {
    font-size: 14px;
    color: #fb4e44;
}
.goods .foods-wrapper .food-list .food-item .content .price .unit {
    font-size: 12px;
    color: #bfbfbf;
}

/* 当前选中 */
.goods .menu-wrapper .menu-item.current {
    background: white;
    font-weight: bold;
    margin-top: -1px;
}
.goods .menu-wrapper .menu-item:first-child.current {
    margin-top: 1px;
}

.goods .foods-wrapper .food-list .food-item .cartcontrol-wrapper {
    position: absolute;
    right: 0;
    bottom: 0;
}

.goods .menu-wrapper .menu-item .num {
    position: absolute;
    right: 5px;
    top: 5px;
    width: 13px;
    height: 13px;
    border-radius: 50%;
    color: white;
    background: red;
    text-align: center;
    font-size: 7px;
    line-height: 13px;
}
</style>
