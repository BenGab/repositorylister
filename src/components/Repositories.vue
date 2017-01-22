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
                <li class="page-item" v-if="stepperVisibility.prev">
                    <a class="page-link" v-on:click="onPageStep(true)" href="#">Previous</a>
                </li>
                <li class="page-item" v-bind:class="isActive(page)" v-for="page in pageList">
                    <a class="page-link" href="#" v-on:click="doPage(page)">{{page}}</a>
                </li>
                 <li class="page-item" v-if="stepperVisibility.next">
                    <a class="page-link" v-on:click="onPageStep(false)" href="#">Next</a>
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
                currentPage: 1,
                pages: 1
            }
        },
        methods: {
            doPage: function(pageNumber) {
                this.currentPage = pageNumber;
            },
            isActive: function(page) {
                return page === this.currentPage ? 'active' : '';
            },
            onPageStep: function(isPrev) {
                let stepNumber = isPrev ? -1 : 1;
                let nextPage = this.currentPage + stepNumber;
                this.doPage(nextPage);
            }
        },
        computed: {
            repositoriPage: function() {
                let start = (this.currentPage - 1) * this.itemsPerPage;
                let end = start + this.itemsPerPage;
                return this.repositories.slice(start, end);
            },
            pageList: function() {
                var pagesResult = [];
                for(let i = 1; i <= this.pages; i++) {
                    pagesResult.push(i);                
                }

                return pagesResult;
            },
            stepperVisibility: function() {
                return {
                    next: this.currentPage < this.pages,
                    prev: this.currentPage > 1
                };
            } 
        },
        created: function () {
            this.$http.get('https://api.github.com/users/addyosmani/repos')
                .then(function(response) {
                    this.repositories = response.data
                    this.pages = Math.ceil(this.repositories.length / this.itemsPerPage);
                });

        }
    }
</script>