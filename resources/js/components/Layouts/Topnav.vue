<template>
    <div class="top-nav bg-light">
        <div class="container">
            <div class="row justify-content-between">
                <div class="col-md-6 d-flex">
                    <div class="bonus-login">
                        <div v-if="user" class="dropdown">
                            <span class="dropdown-toggle text-capitalize" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
                                {{user.name}}
                            </span>
                            <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
                                <li><router-link to="/profile" class="dropdown-item text-capitalize">{{$t('profile')}}</router-link></li>
                                <li><a href="#" @click.prevent="logout()" class="dropdown-item text-capitalize">{{$t('logout')}}</a></li>
                            </ul>
                        </div>

                        <span v-if="!user" @click="showModal()" class="cursor-pointer text-capitalize">
                            <i class="fa fa fa-address-card" aria-hidden="true"></i>
                            {{$t('login')}}
                            {{$t('or')}}
                            {{$t('registration')}}
                        </span>
                    </div>

                    <div class="bonus-language">
                        <div class="dropdown">
                            <span class="dropdown-toggle text-capitalize" type="button" id="dropdownMenuLanguage" data-bs-toggle="dropdown" aria-expanded="false">
                                {{$t('lang')}}
                            </span>
                            <ul class="dropdown-menu" aria-labelledby="dropdownMenuLanguage">
                                <li>
                                    <a href="#" @click.prevent="langChange('ar')" class="dropdown-item text-capitalize">
                                        <img class="flag" src="https://img.icons8.com/color/48/000000/egypt-circular.png"/>
                                        {{$t('arabic')}}
                                    </a>
                                </li>
                                <li>
                                    <a href="#" @click.prevent="langChange('en')" class="dropdown-item text-capitalize">
                                        <img class="flag" src="https://img.icons8.com/color/48/000000/great-britain-circular.png"/>
                                        {{$t('english')}}
                                    </a>
                                </li>
                                <li>
                                    <a href="#" @click.prevent="langChange('de')" class="dropdown-item text-capitalize">
                                        <img class="flag" src="https://img.icons8.com/color/48/000000/germany-circular.png"/>
                                        {{$t('deutsch')}}
                                    </a>
                                </li>
                                <li>
                                    <a href="#" @click.prevent="langChange('ru')" class="dropdown-item text-capitalize">
                                        <img class="flag" src="https://img.icons8.com/color/48/000000/russian-federation-circular.png"/>
                                        {{$t('русский')}}
                                    </a>
                                </li>
                                <li>
                                    <a href="#" @click.prevent="langChange('pl')" class="dropdown-item text-capitalize">
                                        <img class="flag" src="https://img.icons8.com/color/48/000000/poland-circular.png"/>
                                        {{$t('polski')}}
                                    </a>
                                </li>
                            </ul>
                        </div>
                    </div>

                    <div class="bonus-currency">
                        <div class="dropdown">
                            <span class="dropdown-toggle text-capitalize" type="button" id="dropdownMenuLanguage" data-bs-toggle="dropdown" aria-expanded="false">
                                {{$t('currency')}}
                            </span>
                            <ul class="dropdown-menu" aria-labelledby="dropdownMenuLanguage">
                                <li v-for="c in currencies" :key="c.id">
                                    <a href="#" @click.prevent="currencyChange(c)" class="dropdown-item text-capitalize">
                                        {{ $t(c.abbr) }}
                                    </a>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>

                <div class="col-md-6 d-none d-md-flex justify-content-end">
                    <div class="bonus-mail">
                        <a target="_blank" :href="'mailto:'+sittings.email">{{ sittings.email }}</a>
                    </div>
                    <ul class="bonus-social nav">
                        <li v-for="s in social" :key="s.id">
                            <a v-if="s.key && s.key === 'facebook'" target="_blank" :href="s.value" :title="$t('facebook')"><span class="fa fa-facebook"></span></a>
                            <a v-if="s.key && s.key === 'twitter'" target="_blank" :href="s.value" :title="$t('twitter')"><span class="fa fa-twitter"></span></a>
                            <a v-if="s.key && s.key === 'whatsApp'" target="_blank" :href="'https://api.whatsapp.com/send?phone='+s.value" :title="$t('whatsApp')"><span class="fa fa-whatsapp"></span></a>
                            <a v-if="s.key && s.key === 'linkedIn'" target="_blank" :href="s.value" :title="$t('linkedin')"><span class="fa fa-linkedin"></span></a>
                            <a v-if="s.key && s.key === 'pinterest'" target="_blank" :href="s.value" :title="$t('pinterest')"><span class="fa fa-pinterest"></span></a>
                            <a v-if="s.key && s.key === 'instagram'" target="_blank" :href="s.value" :title="$t('instagram')"><span class="fa fa-instagram"></span></a>
                            <a v-if="s.key && s.key === 'youtube'" target="_blank" :href="s.value" :title="$t('youtube')"><span class="fa fa-youtube"></span></a>
                        </li>
                    </ul>
                </div>
            </div>

        </div>

        <!-- auth modal  -->
        <modal :scrollable="true" width="350px" height="auto" name="authModal">
            <div class="tt_popup_modal">
                <i @click="closeModal()" class="fa fa-times-circle close-modal"></i>
                <strong>
                    <i class="fa fa fa-address-card" aria-hidden="true"></i>
                    {{$t('login')}}
                    {{$t('or')}}
                    {{$t('registration')}}
                </strong>
            </div>

            <div class="auth-modal">
                <ul class="nav nav-tabs">
                    <li class="nav-item w-50">
                        <button
                            @click="switchFormAuth('registration')"
                            :class="registration ? 'active': ''"
                            class="nav-link text-capitalize w-100">
                            <i class="fa fa-address-card mr-1"></i>
                            {{$t('registration')}}
                        </button>
                    </li>
                    <li class="nav-item w-50">
                        <button
                            @click="switchFormAuth('login')"
                            :class="login ? 'active': ''"
                            class="nav-link text-capitalize w-100">
                            <i class="fa fa-sign-in mr-1"></i>
                            {{$t('login')}}
                        </button>
                    </li>
                </ul>


                <form v-if="registration" @submit.prevent="registrationFun()" class="p-3 border">

                    <div class="mb-3">
                        <label for="name" class="form-label text-capitalize">{{$t('name')}}</label>
                        <input type="text" :class="{'is-invalid': authForm.errors.has('name')}" v-model="authForm.name" class="form-control" id="name" :placeholder="$t('name')">
                        <div class="invalid-feedback"
                              v-if="authForm.errors.has('name')"
                              v-html="authForm.errors.get('name')">
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="email" class="form-label text-capitalize">{{$t('email')}}</label>
                        <input type="email" autocomplete="off" :class="{'is-invalid': authForm.errors.has('email')}" v-model="authForm.email" class="form-control" id="email" :placeholder="$t('email')">
                        <div class="invalid-feedback"
                             v-if="authForm.errors.has('email')"
                             v-html="authForm.errors.get('email')">
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="phone" class="form-label text-capitalize">{{$t('phone')}}</label>
                        <VuePhoneNumberInput id="phone" :default-country-code="'EG'" :show-code-on-list="true" @update="updatedddd($event)"
                          :placeholder="$t('phone')" v-model="authForm.phone" :class="{'is-invalid': authForm.errors.has('phone')}"
                        />
                        <div class="invalid-feedback"
                              v-if="authForm.errors.has('phone')"
                              v-html="authForm.errors.get('phone')">
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="password" class="form-label text-capitalize">{{$t('password')}}</label>
                        <input type="password" autocomplete="new-password" :class="{'is-invalid': authForm.errors.has('password')}" v-model="authForm.password" class="form-control" id="password" :placeholder="$t('password')">
                        <div class="invalid-feedback"
                              v-if="authForm.errors.has('password')"
                              v-html="authForm.errors.get('password')">
                        </div>
                    </div>

                    <div class="mb-3">
                        <label for="password_confirmation" class="form-label text-capitalize">{{$t('password_confirmation')}}</label>
                        <input type="password" :class="{'is-invalid': authForm.errors.has('password_confirmation')}" v-model="authForm.password_confirmation" class="form-control" id="password_confirmation" :placeholder="$t('password_confirmation')">
                        <div class="invalid-feedback"
                              v-if="authForm.errors.has('password_confirmation')"
                              v-html="authForm.errors.get('password_confirmation')">
                        </div>
                    </div>

                    <div class="mb-3 text-center">
                        <button type="submit" class="btn button btn-info">
                            <span v-if="authForm.busy" class="spinner-border spinner-border-sm"></span>
                            <i v-if="!authForm.busy" class="fa fa-address-card mr-1"></i>
                            {{$t('registration')}}
                        </button>
                    </div>
                </form>

                <form v-if="login" @submit.prevent="loginFun()" class="p-3 border">

                    <div class="mb-3">
                        <label for="login-email" class="form-label text-capitalize">{{$t('email')}}</label>
                        <input type="email" autocomplete="off" :class="{'is-invalid': authForm.errors.has('email')}" v-model="authForm.email" class="form-control" id="login-email" :placeholder="$t('email')">
                        <div class="invalid-feedback"
                             v-if="authForm.errors.has('email')"
                             v-html="authForm.errors.get('email')">
                        </div>
                    </div>


                    <div class="mb-3">
                        <label for="login-password" class="form-label text-capitalize">{{$t('password')}}</label>
                        <input type="password" autocomplete="new-password" :class="{'is-invalid': authForm.errors.has('password')}" v-model="authForm.password" class="form-control" id="login-password" :placeholder="$t('password')">
                        <div class="invalid-feedback"
                              v-if="authForm.errors.has('password')"
                              v-html="authForm.errors.get('password')">
                        </div>
                    </div>

                    <div class="mb-3 text-center">
                        <button type="submit" class="btn button btn-info">
                            <span v-if="authForm.busy" class="spinner-border spinner-border-sm"></span>
                            <i v-if="!authForm.busy" class="fa fa-sign-in mr-1"></i>
                            {{$t('login')}}
                        </button>
                    </div>
                </form>
            </div>
        </modal>
        <!-- end auth modal-->
<!--        <notifications position="bottom left" />-->

    </div>
</template>
<script>
import Form from "vform";
import { mapGetters } from 'vuex';
export default {
name: 'Topnav',
    computed: {
        ...mapGetters([
            'user'
        ])
    },
    data() {
        return {
            social: [],
            sittings: {},
            currencies: [],
            registration: true,
            login: false,
            authForm: new Form({
                name: "",
                email: "",
                phone: "",
                e164Phone: "",
                country: "",
                password: "",
                password_confirmation: ""
            })
        }
    },
    mounted(){
        axios.get('sittings').then((res)=> {
            if(res.data.success === "true"){
                this.sittings = res.data.data ? res.data.data.sittings : {};
                this.social = res.data.data ? res.data.data.social : [];
            }
        }).catch();
        axios.get("langcurrency").then((res) => {
            this.currencies = res.data.data && res.data.data.currencies ? res.data.data.currencies : [];
        }).catch();
    },
    methods: {
        showModal(){
            this.$modal.show('authModal');
        },
        closeModal(){
            this.$modal.hide('authModal');
        },
        switchFormAuth(switcher){
            if (switcher === 'registration'){
                this.registration = true;
                this.login = false;
            }else {
                this.registration = false;
                this.login = true;
            }
        },
        langChange(lang) {
            localStorage.setItem("lang", lang);
            window.location.reload();
        },
        currencyChange(currency) {
            this.$store.dispatch('currency', currency);
        },
        registrationFun() {
            this.authForm.phone = this.authForm.e164Phone;
            this.authForm.post("user/store").then((res) => {
                if (res.data.status === 'success') {
                    this.closeModal();
                    this.$store.dispatch('login', this.authForm);
                } else {
                    this.authForm.errors.set(res.data.data);
                }
            }).catch();
        },
        loginFun(){
            this.authForm.post("user/login").then((res) => {
                if (res.data.status === 'success') {
                    this.closeModal();
                    this.$store.dispatch('login', this.authForm);
                } else {
                    this.authForm.errors.set(res.data.data);
                }
            }).catch();
        },
        updatedddd(event){
            console.log(event);
            this.authForm.e164Phone = event.e164;
            this.authForm.country = event.countryCode;
        },
        logout(){
            this.$store.dispatch('logout');
            this.showModal();
        }
    }
}
</script>
