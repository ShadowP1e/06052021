<template>
    <div class="container mt-5">
            <h1>Список книг нашей библиотеки</h1>
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Название</th>
                        <th scope="col">Автор</th>
                        <th scope="col">Наличие</th>
                        <th scope="col">Действия</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="book in books" :key="book.id">
                        <th scope="row">{{ book.id }}</th>
                        <td>{{ book.title }}</td>
                        <td>{{ book.author }}</td>
                        <td>
                            <v-btn v-if="book.availability" depressed color="primary" v-on:click="changeBookAvailability(book.id)">
                                Доступна
                            </v-btn>
                            <v-btn v-else depressed color="primary" v-on:click="changeBookAvailability(book.id)">
                                Выдана
                            </v-btn>
                        </td>
                        <td>
                            <v-btn depressed color="error" v-on:click="deleteBook(book.id)">
                                Удалить
                            </v-btn>
                        </td>
                    </tr>

                    <!-- Строка с полями для добавления новой книги -->
                    <tr>
                        <th scope="row">Добавить</th>
                        <td><input type="text" class="form-control" v-model="title"></td>
                        <td><input type="text" class="form-control" v-model="author"></td>
                        <td></td>
                        <td>
                            <v-btn depressed color="primary" v-on:click="addBook">
                                Добавить
                            </v-btn>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
</template>


<script>
export default {
    data() {
        return {
            title: '',
            author: '',
            id: null,
            books: [],
        }
    },
    methods: {
                loadBookList(){
                    this.$axios.get('http://localhost:8000/api/book/all')
                        .then(res => {
                            console.log(res.data)
                            this.books = res.data
                    })
                },
                addBook(){
                    this.$axios.post('http://localhost:8000/api/book/add', {
                        title: this.title,
                        author: this.author
                    })
                    .then((response) => {
                        console.log(response);
                        this.loadBookList();
                    })
                },
                deleteBook(id){
                    this.$axios.get('http://localhost:8000/api/book/delete/' + id)
                        .then(res => {
                            this.loadBookList();
                    })
                },
                changeBookAvailability(id){
                    this.$axios.get('http://localhost:8000/api/book/change_availabilty/' + id)
                        .then(res => {
                            this.loadBookList();
                    })
                }
            },
    mounted(){
         // Сразу после загрузки страницы подгружаем список книг и отображаем его
            this.loadBookList();
        }
    }
</script>