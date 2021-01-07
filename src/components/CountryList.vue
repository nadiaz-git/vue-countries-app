<template>
    <b-container class="container vlg-bg">
        <b-row class="bg-light-header">
            <b-col sm="3">
                <p class="bold" style="font-size: 20px;">Where in the world?</p>
            </b-col>
            <b-col sm="3" offset-sm="6">
                <p class="light"><font-awesome-icon icon="moon" />&nbsp;Dark Mode</p>
            </b-col>
        </b-row>
        <div class="bg-light">
            <b-col v-show="!isDetailView">
                <b-row>
                    <b-col cols="12" sm="3" class="margin30">
                        <div class="input-group">
                            <input type="text" class="form-control" v-model="countryFilter" placeholder="Search for a country..." @input="getCountryByName(countryFilter)">
                        </div>
                    </b-col>
                    <b-col cols="12" sm="2" offset-sm="7" class="margin30">
                        <b-dropdown id="dropdown-1" text="Filter by region" class="m-md-2">
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
            <b-col>
                <div id="app">
                    <b-row>
                        <div class="col-xs-12 col-sm-6 col-md-3 card"
                             v-show="!isDetailView"
                             v-for="country in countries"
                             v-bind:key="country.numericCode"
                             @click="showCountryDetail(country)">
                            <b-row>
                                <div class="col-sm-12">
                                    <img class="image-style" v-bind:src="country.flag"/>
                                </div>
                                <div class="col-sm-12">
                                    <p class="bold">{{country.name}}</p>
                                    <p class="bold">Population: <span>{{country.population.toLocaleString()}}</span></p>
                                    <p class="bold">Region: <span>{{country.region}}</span></p>
                                    <p class="bold">Capital: <span>{{country.capital}}</span></p>
                                </div>
                            </b-row>
                        </div>
                        <div class="col-sm-12" v-if="isDetailView && countryInfo">
                            <b-row class="margin30">
                                <b-col>
                                    <b-button @click="backToMainPage()">Back</b-button>
                                </b-col>
                            </b-row>
                            <b-row class="margin30">
                                <b-col>
                                    <img class="image-style" v-bind:src="countryInfo.flag"/>
                                </b-col>
                                <b-col>
                                    <b-row>
                                        <b-col>
                                            <p class="bold">{{countryInfo.name}}</p>
                                            <p class="bold">Native Name: <span>{{countryInfo.nativeName}}</span></p>
                                            <p class="bold">Population: <span>{{countryInfo.population.toLocaleString()}}</span></p>
                                            <p class="bold">Region: <span>{{countryInfo.region}}</span></p>
                                            <p class="bold">Sub Region: <span>{{countryInfo.subregion}}</span></p>
                                            <p class="bold">Capital: <span>{{countryInfo.capital}}</span></p>
                                        </b-col>
                                        <b-col>
                                            <p class="bold">Top Level Domain:
                                                <span v-for="(domain, idx) in countryInfo.topLevelDomain"
                                                       v-bind:key="domain">
                                                    {{ domain + (idx === countryInfo.topLevelDomain.length - 1 ? '' : ',')}}
                                                </span>
                                            </p>
                                            <p class="bold">Currencies:
                                                <span v-for="(currency, idx) in countryInfo.currencies"
                                                       v-bind:key="currency.name">
                                                    {{ currency.name + (idx === countryInfo.currencies.length - 1 ? '' : ',')}}
                                                </span>
                                            </p>
                                            <p class="bold">Languages:
                                                <span v-for="(lang, idx) in countryInfo.languages"
                                                       v-bind:key="lang">
                                                    {{ lang.name + (idx === countryInfo.languages.length - 1 ? '' : ',') }}
                                                </span>
                                            </p>
                                        </b-col>
                                    </b-row>
                                    <b-row>
                                        <b-col>
                                            <p class="bold">Border Countries:</p>
                                            <b-button v-for="border in countryInfo.borders"
                                                    v-bind:key="border" style="margin: 0 0 5px 5px;"
                                                    @click="showBorderCountry(border)">
                                                {{ border }}
                                            </b-button>
                                        </b-col>
                                    </b-row>
                                </b-col>
                            </b-row>
                        </div>
                    </b-row>
                </div>
            </b-col>
        </div>
    </b-container>
</template>

<script>
    export default {
        name: "CountryList",
        props: ['country'],
        data () {
            return {
                countries: null,
                countryFilter: '',
                regionList: ['Africa', 'Americas', 'Asia', 'Europe', 'Oceania'],
                isDetailView: false,
                countryInfo: null
            }
        },
        mounted () {
            this.getAllCountries();
        },
        methods: {
            getCountryByName(countryFilter) {
                if (countryFilter) {
                    this.$http.get('https://restcountries.eu/rest/v2/name/' + countryFilter)
                        .then(response => (this.countries = response.data));
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
            }
        }
    }
</script>

<style scoped>
    .image-style {
        max-width: 250px;
        max-height: 250px;
    }

    .margin30 {
        margin-top: 30px;
    }

    .card {
        cursor: pointer;
        padding: 0 15px 15px 0;
    }

</style>