<template>
    <div id="app">
        <b-container class="container">
            <!--header-->
            <b-row id="titleBar" v-bind:class="darkMode ? 'darktheme-h' : 'lighttheme-h'">
                <b-col cols="8" sm="3">
                    <h3 class="bold" style="font-size: 20px;">Where in the world?</h3>
                </b-col>
                <b-col cols="4" sm="3" offset-sm="6">
                    <p class="light" style="text-align: right;cursor: pointer;" @click="toggleTheme()">
                        <font-awesome-icon icon="sun" v-show="darkMode" />
                        <font-awesome-icon icon="moon" v-show="!darkMode" />&nbsp;{{darkMode ? 'Light' : 'Dark'}} Mode
                    </p>
                </b-col>
            </b-row>
            <!--end header-->

            <!--content-->
            <b-row v-bind:class="darkMode ? 'darktheme' : 'lighttheme'">
                <!--search bar-->
                <b-col v-show="!isDetailView" cols="12">
                    <b-row id="searchBar">
                        <b-col cols="12" sm="4" style="margin-bottom: 30px;">
                            <div class="input-group">
                                <input type="text" class="form-control" v-model="countryFilter"
                                       placeholder="Search for a country..." @input="getCountryByName(countryFilter)">
                            </div>
                        </b-col>
                        <b-col cols="12" sm="2" offset-sm="6" style="margin-bottom: 30px;">
                            <b-dropdown id="dropdown-1" text="Filter by Region" class="m-md-2">
                                <b-dropdown-item
                                        v-for="region in regionList"
                                        v-bind:key="region"
                                        @click="getCountryByRegion(region)">
                                    {{region}}
                                </b-dropdown-item>
                            </b-dropdown>
                        </b-col>
                    </b-row>
                </b-col>
                <!--end search bar -->

                <!--content page-->
                <b-col cols="12">
                    <div id="contentdiv">
                        <b-row v-show="!isDetailView" id="maindiv">
                            <b-col cols="12" v-show="!countries.length">
                                <p>No Result</p>
                            </b-col>
                            <b-col v-for="country in countries" cols="12" sm="3"
                                   v-bind:key="country.numericCode">
                                <b-card v-bind:img-src="country.flag"
                                        @click="showCountryDetail(country)"
                                        img-alt="Image"
                                        img-top
                                        tag="article"
                                        class="cardstyle"
                                        v-bind:class="darkMode ? 'darkcard' : 'lightcard'">
                                    <b-card-text>
                                        <h5 class="bold">{{country.name}}</h5>
                                        <p class="light">Population:
                                            <span>{{country.population.toLocaleString()}}</span>
                                            <br>
                                            Region: <span>{{country.region}}</span>
                                            <br>
                                            Capital: <span>{{country.capital}}</span>
                                        </p>
                                    </b-card-text>
                                </b-card>
                            </b-col>
                        </b-row>

                        <b-row v-if="isDetailView && countryInfo" id="detaildiv">
                            <b-col>
                                <b-row class="margin30">
                                    <b-col>
                                        <b-button @click="backToMainPage()"><font-awesome-icon icon="arrow-left" />&nbsp;Back</b-button>
                                    </b-col>
                                </b-row>
                                <b-row class="margin30">
                                    <b-col cols="12" sm="6" style="margin-bottom: 30px;">
                                        <img class="image-style2" v-bind:src="countryInfo.flag"/>
                                    </b-col>
                                    <b-col cols="12" sm="6" style="margin-bottom: 30px;">
                                        <b-row>
                                            <b-col cols="12">
                                                <h4 class="bold">{{countryInfo.name}}</h4>
                                            </b-col>
                                            <b-col cols="12" sm="6">
                                                <p class="light">
                                                    Native Name: <span>{{countryInfo.nativeName}}</span>
                                                    <br>
                                                    Population: <span>{{countryInfo.population.toLocaleString()}}</span>
                                                    <br>
                                                    Region: <span>{{countryInfo.region}}</span>
                                                    <br>
                                                    Sub Region: <span>{{countryInfo.subregion}}</span>
                                                    <br>
                                                    Capital: <span>{{countryInfo.capital}}</span>
                                                </p>
                                            </b-col>
                                            <b-col cols="12" sm="6">
                                                <p class="light">Top Level Domain:
                                                    <span v-for="(domain, idx) in countryInfo.topLevelDomain"
                                                          v-bind:key="domain">
                                                    {{ domain + (idx === countryInfo.topLevelDomain.length - 1 ? '' : ',')}}
                                                    </span>
                                                    <br>
                                                    Currencies: <span v-for="(currency, idx) in countryInfo.currencies"
                                                          v-bind:key="currency.name">
                                                    {{ currency.name + (idx === countryInfo.currencies.length - 1 ? '' : ',')}}
                                                    </span>
                                                    <br>
                                                    Languages:
                                                    <span v-for="(lang, idx) in countryInfo.languages"
                                                          v-bind:key="lang">
                                                    {{ lang.name + (idx === countryInfo.languages.length - 1 ? '' : ',') }}
                                                    </span>
                                                </p>
                                            </b-col>
                                        </b-row>
                                        <b-row>
                                            <b-col>
                                                <p class="light">Border Countries:</p>
                                                <b-button v-for="border in countryInfo.borders"
                                                          v-bind:key="border" style="margin: 0 0 5px 5px;"
                                                          @click="showBorderCountry(border)">
                                                    {{ border }}
                                                </b-button>
                                            </b-col>
                                        </b-row>
                                    </b-col>
                                </b-row>
                            </b-col>
                        </b-row>
                    </div>
                </b-col>
                <!--end detail page-->
            </b-row>
            <!--end content-->
        </b-container>
    </div>
</template>

<script>
    export default {
        name: "CountryList",
        props: ['country'],
        data () {
            return {
                countries: [],
                countryFilter: '',
                regionList: ['Africa', 'Americas', 'Asia', 'Europe', 'Oceania'],
                isDetailView: false,
                countryInfo: null,
                darkMode: false
            }
        },
        mounted () {
            this.getAllCountries();
        },
        methods: {
            getCountryByName(countryFilter) {
                if (countryFilter) {
                    this.$http.get('https://restcountries.eu/rest/v2/name/' + countryFilter)
                        .then(response => (this.countries = response.data))
                        .catch(error => {
                            this.countries = [];
                            console.log(error);
                        });
                } else {
                    this.getAllCountries();
                }
            },
            getAllCountries() {
                this.$http.get('https://restcountries.eu/rest/v2/all')
                    .then(response => (this.countries = response.data));
            },
            getCountryByRegion(region) {
                this.$http.get('https://restcountries.eu/rest/v2/region/' + region)
                    .then(response => (this.countries = response.data));
            },
            showCountryDetail(country) {
                this.isDetailView = true;
                this.countryInfo = country;
            },
            backToMainPage() {
                this.isDetailView = false;
            },
            showBorderCountry(border) {
                this.$http.get('https://restcountries.eu/rest/v2/alpha/' + border)
                    .then(response => (this.countryInfo = response.data));
            },
            toggleTheme() {
                this.darkMode = !this.darkMode;
            }
        }
    }
</script>

<style scoped>
    .image-style {
        max-width: 100px;
        max-height: 100px;
    }

    .image-style2 {
        max-width: 100%;
        max-height: 375px;
    }

    .margin30 {
        margin-top: 30px;
    }

    #searchBar {
        margin: 30px 15px 10px 15px;
    }

    #titleBar {
        padding: 30px 30px 0 30px;
    }

    #contentdiv {
        margin: 0 30px 30px 30px;
    }

    #maindiv p {
        font-size: 14px;
    }

    #detaildiv p {
        font-size: 16px;
    }

</style>