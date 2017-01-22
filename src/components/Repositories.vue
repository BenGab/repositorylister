<template>
	<div class="repositories">
		<h1>{{title}}</h1>
        <div class="container">
            <div class="row" v-for="repositori in repositoriPage">
                <div class="col-md-4">
                    {{repositori.name}}
                </div>
                <div class="col-md-4">
                    {{repositori.description}}
                </div>
                <div class="col-md-2">
                    <a v-bind:href="repositori.html_url" target="blank">Git url</a>
                </div>
            </div>
        </div>
        <nav aria-label="...">
            <ul class="pagination pagination-lg">
                <li class="page-item" v-bind:class="isActive(page)" v-for="page in pages">
                    <a class="page-link" href="#" v-on:click="doPage(page)">{{page}}</a>
                </li>
            </ul>
        </nav>
	</div>
</template>
<script>
    export default {
        name: 'repositories',
        props: {
            itemsPerPage: {
                type: Number,
                default: 5
            }
        },
        data() {
            return {
                title: 'Repository list of Addy Osmani',
                repositories: [],
                currentPage: 1
            }
        },
        methods: {
            doPage: function(pageNumber) {
                this.currentPage = pageNumber;
            },
            isActive: function(page) {
                return page === this.currentPage ? 'active' : '';
            }
        },
        computed: {
            repositoriPage: function() {
                let start = (this.currentPage - 1) * this.itemsPerPage;
                let end = start + this.itemsPerPage;
                return this.repositories.slice(start, end);
            },
            pages: function() {
                var pagesResult = [];
                var pages = Math.ceil(this.repositories.length / this.itemsPerPage);
                for(let i = 0; i != pages; i++) {
                    pagesResult.push(i);
                }

                return pages;
            }   
        },
        created: function () {
            this.$http.get('https://api.github.com/users/addyosmani/repos')
                .then(function(response) {
                    this.repositories = response.data
                    console.log(response.body);
                });

        }
    }
</script>