<template>
    <main>
        <div class="char__content">
            <ul class="char__grid">
                <li class="char__item" v-for="item in itemPosts" :key="item.id">
                    <div class="char__item_header">
                        <div class="char__item_name">
                            <span>{{item.name}}</span>
                            {{item.gender}}
                        </div>
                        <div class="char__item_status">
                            <span v-if="item.status === 'Dead'" :style="{color: '#E60000'}"
                            >{{item.status}}</span>
                            <span v-if="item.status === 'Alive'" :style="{color: '#00BD4C'}"
                            >{{item.status}}</span>
                            <span v-if="item.status === 'unknown'" :style="{color: '#7B7B7B'}"
                            >{{item.status}}</span>
                            {{item.origin.name}}
                        </div>
                    </div>
                    <img :src="item.image" :alt="item.name">
                </li>
            </ul>
        </div>
        <div class="char__pagination">
            <div class="char__pagination_number">
                <span class="char__pagination_start" @click="pageStart"
                :style="{display: page === 1 ? 'none' : null}">1</span>
                ...    
                <span class="char__pagination_before" @click="pagePrev"
                :style="{display: prevPage === 0 ? 'none' : null}">{{prevPage}}</span>    
                <span class="current_page" :style="{cursor: 'default'}">{{page}}</span>    
                <span class="char__pagination_after" @click="pageNext"
                :style="{display: nextPage === pagesTotal + 1 ? 'none' : null}">{{nextPage}}</span>    
                ...    
                <span class="char__pagination_end" @click="pageEnd"
                :style="{display: page === pagesTotal ? 'none' : null}">{{pagesTotal}}</span>
            </div>
        </div>
    </main>
</template>

<script>
import axios from 'axios';

export default {
  name: "MyComponent",
  data() {
    return {
        posts: [],
        itemPosts: [],
        page: 1,
        prevPage: 0,
        nextPage: 2,
        itemsLimit: 4,
        pagesTotal: 0,
        minId: 0,
        maxId: 5,
        fetchPage: 1,
        count: 0
    }
  },
  methods: {
    async fetchRequest(num) {
        try {
            const response = await axios.get(`https://rickandmortyapi.com/api/character/?page=${num}`)
            this.posts = response.data.results
            this.pagesTotal = Math.ceil(response.data.info.count / this.itemsLimit)
            this.count = response.data.info.count
            this.postsItem();
        } catch (error) {
            console.log('Error');
        }
    },
    postsItem() {
        this.itemPosts = this.posts.filter(item => item.id < this.maxId && item.id > this.minId)
        console.log(this.itemPosts);
    },
    pageStart() {
        this.page = 1;
        this.prevPage = 0;
        this.nextPage = 2;
        this.minId = 0; this.maxId = 5;
        this.fetchRequest(this.page)
    },
    pagePrev() {
        if (this.page > 1) {
            this.page -= 1;
            this.prevPage -= 1;
            this.nextPage -= 1;
            this.minId -= 4; this.maxId -= 4;
            
            if (this.page % 5 === 0) {
                this.fetchPage -= 1;
                this.fetchRequest(this.fetchPage);
            } else {
                this.postsItem();
            }
        }
    },
    pageNext() {
        if (this.page < this.pagesTotal) {
            this.page += 1;
            this.prevPage += 1;
            this.nextPage += 1;
            this.minId += 4; this.maxId += 4;

            if ((this.page + 1) % 5 === 1) {
                this.fetchPage += 1;
                this.fetchRequest(this.fetchPage);
            } else {
                this.postsItem();
            }
        }
    },
    pageEnd() {
        this.page = this.pagesTotal;
        this.prevPage = this.page - 1;
        this.nextPage = this.pagesTotal + 1;
        this.minId = this.count - 4; this.maxId = this.count + 1;
        this.fetchRequest(Math.ceil(this.pagesTotal / 5));
    }
  },
  mounted() {
    this.fetchRequest(this.page);
  }
};
</script>

<style>
    @media (min-width: 320px) {
        main {
        padding: 15px 0px;
        position: relative;
        }
        .char__content {
        max-width: 300px;
        margin: 0 auto;
        }
        .char__grid {
        display: -ms-grid;
        display: grid;
        grid-template-columns: 300px;
        grid-template-rows: repeat(2, 330px);
        gap: 30px;
        }
        .char__item img {
        width: 300px;
        height: 300px;
        -o-object-fit: cover;
            object-fit: cover;
        }
        .char__item:hover {
        -webkit-box-shadow: 5px 10px 20px rgba(0, 0, 0, 0.25);
                box-shadow: 5px 10px 20px rgba(0, 0, 0, 0.25);
        -webkit-transition: .4s all;
        transition: .4s all;
        }
        .char__item_header {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: justify;
            -ms-flex-pack: justify;
                justify-content: space-between;
        height: 30px;
        font-weight: 400;
        font-style: normal;
        font-size: 12px;
        line-height: 14px;
        color: #8D8D8D;
        }
        .char__item_name {
        text-align: start;
        }
        .char__item_name span {
        display: block;
        font-weight: 700;
        color: #333333;
        }
        .char__item_status {
        text-align: end;
        }
        .char__item_status span {
        display: block;
        font-weight: 700;
        color: #00BD4C;
        }
        .char__item_min {
        display: none;
        }
        .char__pagination {
        display: block;
        position: absolute;
        left: 0;
        bottom: 0;
        height: 60px;
        width: 100%;
        background: #000000;
        opacity: 0.9;
        }
        .char__pagination_number {
        height: 100%;
        margin: 0 auto;
        color: #FFFFFF;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 60px;
        text-align: center;
        }
        .char__pagination_number span {
        margin: 0 10px;
        text-decoration: none;
        color: inherit;
        cursor: pointer;
        }
        .current_page {
        font-weight: 700;
        font-size: 18px;
        }
    }
    
    @media (min-width: 480px) {
        .char__pagination_number {
        width: 320px;
        }
    }
    
    @media (min-width: 768px) {
        main {
        padding: 19px 0px 11px 0px;
        }
        .char__content {
        max-width: 630px;
        }
        .char__grid {
            grid-template-columns: repeat(2, 300px);
        }
        .char__item_min {
        display: block;
        }
    }
    
    @media (min-width: 1000px) {
        main {
        padding: 16px 0 14px 0;
        }
    }
</style>