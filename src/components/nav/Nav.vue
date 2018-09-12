<template>
    <div class="nav">

        <router-link to="/goods" class="nav-item">
            点菜
            <i class="line"></i>
        </router-link>

        <router-link to="/ratings" class="nav-item">
            评价({{commentNum}})
            <i class="line"></i>
        </router-link>

        <router-link to="/seller" class="nav-item">
            商家
            <i class="line"></i>
        </router-link>

    </div>
</template>

<script>
export default {
    props: {
        commentNum: {
            type: Number,
            default() {
                return 0;
            }
        }
    }
};
</script>

<style scoped>
.nav {
    display: flex;
    width: 100%;
    height: 43px;
    line-height: 43px;
    border-bottom: 1px solid #e4e4e4;
}
.nav-item {
    flex: 1;
    text-align: center;
    text-decoration: none;
    color: #666666;
    position: relative;
}

.nav .active {
    color: #ffbb22;
}

.nav .active .line {
    width: 3rem;
    height: 1px;
    display: inline-block;
    background: #ffbb22;
    position: absolute;
    left: 50%;
    bottom: 0;
    margin-left: -1.5rem;
}
</style>