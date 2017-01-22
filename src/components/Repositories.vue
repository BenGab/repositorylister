<template>
	<div class="repositories">
		<h1>{{title}}</h1>
	</div>
</template>
<script>
    export default {
        name: 'repositories',
        data() {
            return {
                title: 'Repository list of Addy Osmani'
            }
        },
        created: function () {
            this.$http.get('https://api.github.com/users/addyosmani/repos')
                .then(function(response) {
                    console.log(response.body)
                })

        }
    }
</script>