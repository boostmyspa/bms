<template>
  <div style="height:100%">
<nav class="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
            <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="#">BoostMySpa</a>
            <input class="form-control form-control-dark w-100 navigation-url-bar" type="text" placeholder="Change URL" aria-label="Search">
            <ul class="navbar-nav px-3" style="flex-direction:initial!important;">

                <li class="nav-item text-nowrap" style="margin-right:5px">
                    <div v-if="activeCompany >= 0 && companies">
                          <b-button id="id_password"  style="padding: 0 4px;background: inherit!important; border:inherit!important;" variant="primary"><span class="oi oi-key"></span></b-button>
                          <b-popover target="id_password" triggers="click blur" placement="bottom">
                            <template slot="title">Password Details</template>
                              <strong>Username </strong> {{ companies[activeCompany].user }}<br>
                              <strong>Password </strong> {{ companies[activeCompany].password }}
                              <b-button block variant="primary" @click="setPassword">Set Password</b-button>
                          </b-popover>
                    </div>
                </li>

                <li class="nav-item text-nowrap" style="margin-right:5px">
                    <div v-if="activeCompany >= 0 && companies">
                  <b-button id="id_pricing"  style="padding: 0 4px;background: inherit!important;border:inherit!important;" variant="primary"><span class="oi oi-dollar"></span></b-button>
                  <b-popover target="id_pricing" triggers="click blur" placement="bottom">
                    <template slot="title">Pricing</template>
                      <div v-html="companies[activeCompany].pricing"></div>
                  </b-popover>
                    </div>
                </li>
                <li class="nav-item text-nowrap" style="margin-right:5px">
                    <div v-if="activeCompany >= 0 && companies">
                  <b-button id="id_location"  style="padding: 0 4px;background: inherit!important;border:inherit!important;" variant="primary"><span class="oi oi-map-marker"></span></b-button>
                  <b-popover target="id_location" triggers="click blur" placement="bottom">
                    <template slot="title">Location</template>
                      <div v-html="companies[activeCompany].location"></div>
                  </b-popover>
                    </div>
                </li>
              <li class="nav-item text-nowrap" style="margin-right:5px">
                  <div v-if="activeCompany >= 0 && companies">
                  <b-button id="id_details"  style="padding: 0 4px;background: inherit!important;border:inherit!important;" variant="primary"><span class="oi oi-menu"></span></b-button>
                  <b-popover target="id_details" triggers="click blur" placement="bottom">
                    <template slot="title">General Information</template>

                              <div v-for="notes in companies[activeCompany].notes">
                                  <h6>{{ notes.title}}</h6>
                                  <p>{{notes.description}}</p>
                              </div>

                  </b-popover>
                  </div>
                </li>


            </ul>
        </nav>
<div class="container-fluid" style="height:100%">
  <div class="row"  style="height:100%">
    <nav class="col-md-2 d-none d-md-block bg-light sidebar">
      <div class="sidebar-sticky">

        <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
          <span>Companies</span>
          <a class="d-flex align-items-center text-muted" href="#">
            <span data-feather="plus-circle"></span>
          </a>
        </h6>
        <div class="tabs">
        <ul class="nav flex-column mb-2 tabs-nav">
          <li class="nav-item" v-for="(company, index) in companies" style="margin:0!important">
            <a class="nav-link nav-tab-item" v-bind:class="{ 'active-nav-tab': company.isActive }" @click="setCompany(index)" >
              <span data-feather="file-text"></span>
              {{ company.name }}
            </a>
          </li>
        </ul>
        </div>
      </div>
    </nav>
    <main id="app" role="main" class="col-md-9 ml-sm-auto col-lg-10 px-4" style="padding:0px!important;!important;height:100%;width:100%">
            <div class="tab" style="height:100%;width:100%"
                 v-for="(company, index) in companies"
                 v-bind:style="company.isActive ? 'display:flex':'display:none'">
          <webview @click="":ref="'company-'+index" :src="company.default_url" :partition="'persist:'+company.id" :data-company="company.index" style="width:100%" allowpopups  webpreferences="nativeWindowOpen=yes" ></webview>
                <div v-if="activeCompany < 9999"></div>
                <div v-else>
                    <h3>Welcome to BoostMySpa Panel!</h3>
                </div>
        </div>
    </main>



  </div>
</div>
    </div>
</template>

<script>
  import axios from 'axios'
  import moment from 'moment-timezone'

  export default {
    name: 'HelloWorld',

    data () {
      return {
        activeCompany: 0,
        companies: null
      }
    },
    created () {
      axios.get('https://www.nowmassage.me/api/business').then(response => this.companies = response.data)
    },
    methods: {
      setCompany: function (event) {
        this.companies[this.activeCompany].isActive = false
        this.activeCompany = event
        this.companies[event].isActive = true
      },
      setPassword: function () {
        const ref = 'company-' + this.activeCompany
        const webview = this.$refs[ref][0]

        if (webview.getURL().indexOf('appointment-plus.com') > -1) {
          webview.executeJavaScript('document.getElementById(\'login\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
        if (webview.getURL().indexOf('squareup.com')) {
          alert(2)
          webview.executeJavaScript('document.getElementById(\'email\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
        if (webview.getURL().indexOf('massagebook.com')) {
          webview.executeJavaScript('document.getElementById(\'username\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
        if (webview.getURL().indexOf('massagebook.com')) {
          webview.executeJavaScript('document.getElementById(\'txtUsernameP\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'txtPasswordP\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
        if (webview.getURL().indexOf('acuityscheduling.com')) {
          webview.executeJavaScript('document.getElementsbyName(\'username\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementsbyName(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
        if (webview.getURL().indexOf('mindbodyonline.com')) {
          webview.executeJavaScript('document.getElementById(\'username\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }

        if (webview.getURL().indexOf('mindbodyonline.com')) {
          webview.executeJavaScript('document.getElementById(\'username\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }


        if (webview.getURL().indexOf('mindbodyonline.com')) {
          webview.executeJavaScript('document.getElementById(\'username\').value = \'' + this.companies[this.activeCompany].user + '\'')
          webview.executeJavaScript('document.getElementById(\'password\').value = \'' + this.companies[this.activeCompany].password + '\'')
        }
      },
      setTimezone: function () {
        const ref = 'company-' + this.activeCompany
        const webview = this.$refs[ref][0]
        webview.executeJavaScript('moment.tz.setDefault("America/New_York")')
      }
    }

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>