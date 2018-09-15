<template>
    <div id="app">

        <app-header :poiInfo="poiInfo"></app-header>

        <app-nav :commentNum="commentNum"></app-nav>

        <keep-alive>
            <router-view></router-view>
        </keep-alive>

    </div>
</template>

<script>
import Header from "./components/header/header";
import nav from "./components/nav/nav";
import axios from "axios";

export default {
    name: "App",   
    components: {
        "app-header": Header,
        "app-nav": nav
    },
    data() {
        return {
            poiInfo: {},
            commentNum: 0
        };
    },
    created() {
        // axios
        axios({
            // url: "/api/goods",
            url: "https://www.easy-mock.com/mock/5b9bb6c985a2240a058f4885/mock/goods",
            method: "get"
        })
            .then(response => {
                // console.log(response)
                if (response.status == 200) {
                    this.poiInfo = response.data.data.poi_info;
                    // console.log(this.poiInfo)
                }
            })
            .catch(error => {
                console.log(error);
            });

        axios({
            // url: "/api/ratings",
            url: "https://www.easy-mock.com/mock/5b9bb6c985a2240a058f4885/mock/ratings",
            method: "get"
        })
            .then(response => {
                if (response.status == 200) {
                    this.commentNum = response.data.data.comment_num;
                    // console.log(this.commentNum);
                }
            })
            .catch(error => {
                console.log(error);
            });

        // fetch
        // fetch("/api/goods")
        //     .then(res => {
        //         return res.json();
        //     })
        //     .then(response => {
        //         // console.log(response)
        //         if (response.code == 0) {
        //             // console.log(response.data)
        //             this.poiInfo = response.data.poi_info;
        //             // console.log(this.poiInfo)
        //         }
        //     });
    }
};
</script>

<style scoped>
</style>
