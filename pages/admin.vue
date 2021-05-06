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
                                Добавить
                            </v-btn>
                            <v-btn v-else depressed color="primary" v-on:click="changeBookAvailability(book.id)">
                                Добавить
                            </v-btn>
                        </td>
                        <td>
                            <v-btn depressed color="error" v-on:click="addBook">
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
    async asyncData({ $axios }){
                const list = await $axios.$get('http://127.0.0.1:8000/public/api/book/all');
                console.log(list.data)
                this.books = list.data
        },
    data: {
            title: '',
            author: '',
            id: null,
            books: [],
        },
    mounted(){
         // Сразу после загрузки страницы подгружаем список книг и отображаем его
            this.loadBookList();
        }
    }
</script>