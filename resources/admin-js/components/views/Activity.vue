<template>
    <div class="container-fluid">
		<div class="row">
			<div class="col-12">
				<div class="page-title-box">
					<h4 class="page-title">Activity Log</h4>
				</div>
			</div>
		</div>
        <div class="row overflow-row">
            <div class="col">
                <div class="card">
                    <div class="card-body">
                        <div class="row overflow-row">
                            <div class="col">
                                <a class="text-dark" data-toggle="collapse" aria-expanded="false">
                                    <h5 class="mb-0">Activity</h5>
                                </a>
                                <div class="collapse show">
                                    <div class="card mb-0 shadow-none">
                                        <div class="card-body">
                                            <div class="row justify-content-sm-between border-bottom overflow-row" v-for="entry in activity">
                                                <div class="col-lg-9 mb-2 mb-lg-0 d-flex align-items-center">
                                                    <div class="mr-2">
                                                        <img :src="entry.user.avatar" alt="image" style="width: 30px;" class="me-3 avatar rounded-circle" data-toggle="tooltip" :title="entry.user.name">
                                                    </div>
                                                    {{ entry.user.name }} -&nbsp;<span v-html="entry.html"></span>
                                                </div>
                                                <div class="col-lg-3">
                                                    <div class="d-sm-flex justify-content-between">
                                                        <div class="mt-3 mt-sm-0">
                                                            <ul class="list-inline font-13 text-sm-right">
                                                                <li class="list-inline-item pr-1">
                                                                    {{ entry.time }}
                                                                </li>
                                                            </ul>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        created() {
            axios.post('/admin/activity').then(({ data }) => this.activity = data);
        },
        data() {
            return {
                activity: []
            }
        }
    }
</script>

<style lang="scss">

.overflow-row {
    overflow: auto;
}

</style>