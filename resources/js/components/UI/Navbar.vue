<template>
    <header>
        <nav class="container navbar navbar-expand-lg">
            <logo></logo>


            <button class="navbar-toggler py-2" type="button" data-bs-toggle="collapse"
                    data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
                    aria-expanded="false"
                    aria-label="Toggle navigation">
                <!--                <i class="fa-solid fa-bars fa-xl" style="color: #ffffff;"></i>-->
                <img src="@/img/i-menu.png" width="15" alt="">
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ms-auto mb-2 mb-lg-0">

                    <li class="nav-item dropdown nav-item-active">
                        <a class="nav-link dropdown-toggle px-3" href="#" id="navbarDropdown" role="button"
                           data-bs-toggle="dropdown" aria-expanded="false">
                            Веб-игры
                        </a>
                        <ul class="dropdown-menu " aria-labelledby="navbarDropdown">
                            <li><a class="dropdown-item" href="#">500 злобных карт</a></li>
                            <li><a class="dropdown-item" href="#">Алиас</a></li>
                            <li>
                                <hr class="dropdown-divider">
                            </li>
                            <li>
                                <router-link class="dropdown-item" to="/games"> Все игры</router-link>
                            </li>
                        </ul>
                    </li>

                    <li class="nav-item">
                        <router-link class="nav-link" to="/activities"> Активности</router-link>
                    </li>
                    <li class="nav-item me-4">
                        <router-link class="nav-link mb-md-0 mb-sm-2" to="/about"> О нас</router-link>
                    </li>

                    <li v-if="!token" class="nav-item me-0">
                        <router-link class="nav-link" :to="{name: 'user.login'}">
                            Войти
                        </router-link>
                    </li>
                    <li v-if="!token" class="nav-item me-0">
                        <router-link class="nav-link" :to="{name: 'user.registration'}">
                            Регистрация
                        </router-link>
                    </li>

                    <li class="d-flex">
                        <li v-if="token" class="m-0 me-sm-2 d-flex flex-column justify-content-center">
                            <router-link class="py-0 pe-2" :to="{name: 'user.account'}" > <img src="@/img/navbar/user-solid.svg" width="20" alt="user"> </router-link>
                        </li>
                        <li v-if="token" class=" me-0">
                            <a class="nav-link" href="#" @click.prevent="logout"> Выйти </a>
                        </li>
                    </li>

                </ul>
            </div>
        </nav>
    </header>
</template>

<script>

import axios from "axios";

export default {
    name: 'navbar',
    props: {
        token: {
            type: String
        },
    },
    methods: {
        logout() {
            try {
                axios.get('http://localhost:4008/sanctum/csrf-cookie').then(response => {
                    axios.post('/logout').then(res => {
                        // console.log(res);
                        this.$cookies.remove('x_xsrf_token');
                        this.$parent.getToken();
                        this.$router.push('/');
                    })
                });
            } catch (error) {
                console.log(error.response)
            }

        },
    }
}
</script>

<style scoped lang="scss">
.container {
    background: $color-sub-bg !important;
    margin-top: 0 !important;
    margin-bottom: 0 !important;
    padding: 2px;

    .navbar-toggler {
        background-color: $color-blue !important;
    }

    .navbar-collapse {
        .nav-item-active {
            background: $color-blue;
            transition: 0.2s ease;

            &:hover {
                background: $color-dark-blue;
            }

            .nav-link:hover {
                color: white !important;
            }
        }

        li {
            border-radius: 3px;
            margin-right: 10px;
        }

        .dropdown-item {
            color: #818586 !important;
        }
    }
}

</style>
