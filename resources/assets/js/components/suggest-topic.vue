<style>
</style>

<template>
    <div class="row">
        <div class="col-md-6 col-md-push-3">
            <h2>{{ verb }} a Topic</h2>

            <form @submit.prevent="suggestTopic">
                <label>Title</label><br>
                <input type="text" v-model="title" class="form-control" length="255" autofocus v-el:topic-title-input required><br>

                <label>Description</label><br>
                <textarea v-model="description" class="form-control"></textarea><br>

                <input type="submit" class="btn btn-primary" value="{{ verb }}">
                <a v-link="{ path: '/' }" class="btn btn-default">Cancel</a>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                title: '',
                description: ''
            };
        },
        props: ['topics'],
        methods: {
            suggestTopic: function () {
                var vm = this;

                this.$http.post('topics', { title: this.title, description: this.description }, function (data) {
                    vm.title = '';
                    vm.description = '';

                    vm.$dispatch('topics.created', data);

                    vm.$route.router.go('/');
                });
            }
        },
        computed: {
            verb: function () {
                return Suggestive.isAdmin ? 'Add' : 'Suggest';
            }
        },
        ready: function () {
            this.$els.topicTitleInput.focus();
        }
    };
</script>
