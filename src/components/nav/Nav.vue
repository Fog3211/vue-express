<template>
    <div class="nav">

        <router-link to="/goods" class="nav-item">
            点菜
            <i class="line"></i>
        </router-link>

        <router-link to="/ratings" class="nav-item">
            评价
            <i class="line"></i>
        </router-link>

        <router-link to="/seller" class="nav-item">
            商家
            <i class="line"></i>
        </router-link>

    </div>
</template>

<script>
export default {};
</script>

<style scoped>
.nav {
    display: flex;
    width: 100%;
    height: 4rem;
    line-height: 4rem;
}
.nav-item {
    flex: 1;
    text-align: center;
}
</style>