<template>
    <div class="row">
        <div class="col-12 mb-2">
            <!-- llamamos al componente para Crear   -->
            <router-link :to="{ name: 'createBlog' }" class="btn btn-success"
                ><i class="fas fa-plus-circle"></i
            ></router-link>
        </div>
        <div class="col-12">
            <div class="table-responsive">
                <table class="table table-bordered">
                    <thead class="bg-primary text-white">
                        <tr>
                            <th>ID</th>
                            <th>Título</th>
                            <th>Contenido</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="blog in blogs" :key="blog.id">
                            <td>{{ blog.id }}</td>
                            <td>{{ blog.title }}</td>
                            <td>{{ blog.content }}</td>
                            <td>
                                <!-- llamamos al componente para Editar     -->
                                <router-link
                                    :to="{
                                        name: 'editBlog',
                                        params: { id: blog.id },
                                    }"
                                    class="btn btn-info"
                                    ><i class="fas fa-edit"></i
                                ></router-link>
                                <a
                                    type="button"
                                    @click="deleteBlog(blog.id)"
                                    class="btn btn-danger"
                                    ><i class="fas fa-trash"></i
                                ></a>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "blogs",
    data() {
        return {
            blogs: [],
        };
    },
    mounted() {
        this.showBlogs();
    },
    methods: {
        async showBlogs() {
            await this.axios
                .get("/api/blog")
                .then((response) => {
                    this.blogs = response.data;
                })
                .catch((error) => {
                    console.log(error);
                    this.blogs = [];
                });
        },
        deleteBlog(id) {
            if (confirm("¿Desea eliminar el registro?")) {
                this.axios
                    .delete(`/api/blog/${id}`)
                    .then((response) => {
                        this.showBlogs();
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
        },
    },
};
</script>
