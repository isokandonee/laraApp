<template>
    <div class="container">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Users</h3>
                        <div class="card-tools">
                            <div class="input-group input-group-sm" style="width:150px;" >
                                <input type="text" name="table_search" placeholder="Search..." 
                                    class="form-control float-right">
                                <div class="input-group-append">
                                    <button type="submit" class="fa fa-search" class="btn btn-default"></button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover">
                            <tbody>
                                <tr>
                                    <th>ID</th>
                                    <th>User</th>
                                    <th>Date</th>
                                    <th>Status</th>
                                    <th>Reason</th>
                                </tr>
                                <tr>
                                    <td>183</td>
                                    <td>John Doe</td>
                                    <td>11-07-2014</td>
                                    <td><span class="tag tag-success">Pending</span></td>
                                    <td>ipsum dolor sit amet jndfvnkjkjfdjbv;a jkjdkcb ksjdkj kjsbdkjk</td>
                                </tr>
                                <tr>
                                    <td>183</td>
                                    <td>John Doe</td>
                                    <td>11-07-2014</td>
                                    <td><span class="tag tag-success">Approved</span></td>
                                    <td>ipsum dolor sit amet jndfvnkjkjfdjbv;a jkjdkcb ksjdkj kjsbdkjk</td>
                                </tr>
                                <tr>
                                    <td>183</td>
                                    <td>John Doe</td>
                                    <td>11-07-2014</td>
                                    <td><span class="tag tag-success">Denied</span></td>
                                    <td>ipsum dolor sit amet jndfvnkjkjfdjbv;a jkjdkcb ksjdkj kjsbdkjk</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <!-- .card-body -->
                </div>
                <!-- .card -->
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
