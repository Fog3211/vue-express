<template>
    <div id="app">

        <app-header :poiInfo="poiInfo"></app-header>

        <app-nav :commentNum="commentNum"></app-nav>

        <router-view></router-view>

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
            url: "/api/goods",
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
            url: "/api/ratings",
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

<style>
</style>
