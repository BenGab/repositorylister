<template>
	<div class="repositories">
		<h1>{{title}}</h1>
        <div class="container">
            <div class="row" v-for="repositori in repositories">
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
	</div>
</template>
<script>
    export default {
        name: 'repositories',
        data() {
            return {
                title: 'Repository list of Addy Osmani',
                repositories: []
            }
        },
        created: function () {
            this.$http.get('https://api.github.com/users/addyosmani/repos')
                .then(function(response) {
                    this.repositories = response.data
                    console.log(response.body)
                })

        }
    }
</script>