<template>
  <div>
    <section class="content-header">
      <h1>{{$t('contact.value')}} "{{ item && item['value'] }}"</h1>
    </section>

    <section class="content">
      <item-errors :entity="'contact'"></item-errors>

      <div class="nav-tabs-custom">
        <ul class="nav nav-tabs">
          <li class="active"><a href="#general" data-toggle="tab" aria-expanded="false">{{$t('vat.tabs.general')}}</a></li>
          <li><a href="#history" data-toggle="tab" aria-expanded="false">{{$t('tabs.history')}}</a></li>
        </ul>
        <div class="tab-content">
          <div class="tab-pane active" id="general">
            <div v-if="item" class="table-responsive">
              <table class="table table-striped table-hover" v-if="item">
                <thead>
                <tr>
                  <th>{{$t('field')}}</th>
                  <th>{{$t('value')}}</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                  <td>{{$t('contact.value')}}</td>
                  <td>{{ item['value'] }}</td>
                </tr>
                <tr>
                  <td>{{$t('contact.contactType.name')}}</td>
                  <td>{{ item['contactType']['name'] }}</td>
                </tr>
                <tr>
                  <td>{{$t('contact.clients')}}</td>
                  <td>
                    <ul>
                      <li v-for="client in item['clients']">
                        <router-link :to="{name: 'ClientShow', params: { id: client.id }}">
                          {{ client['name'] }}
                        </router-link>
                      </li>
                    </ul>
                  </td>
                </tr>
                <tr>
                  <td>{{$t('contact.companies')}}</td>
                  <td>
                    <ul>
                      <li v-for="company in item['companies']">
                        <router-link :to="{name: 'CompanyShow', params: { id: company.id }}">
                          {{ company['name'] }}
                        </router-link>
                      </li>
                    </ul>
                  </td>
                </tr>
                <tr>
                  <td>{{$t('contact.createdAt')}}</td>
                  <td>{{ moment(item['createdAt']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>
                <tr>
                  <td>{{$t('contact.updatedAt')}}</td>
                  <td>{{ moment(item['updatedAt']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>
                <tr>
                  <td>{{$t('contact.updatedBy')}}</td>
                  <td>{{ item['updateBy'] }}</td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>
          <div class="tab-pane" id="history">
            <history :id="parseInt($route.params.id)" :entity="'Contact'" :path="'contact'" :key="historyKey"></history>
          </div>
        </div>
      </div>

      <item-show-actions :item="item" :entity="'Contact'" :path="'contact'"></item-show-actions>
    </section>
  </div>
</template>

<script>
  import {mapActions, mapGetters} from 'vuex'
  import ItemShowActions from '../../components/layout/ItemShowActions'
  import ItemErrors from '../../components/layout/errors/ItemErrors'
  import History from '../../components/History'

  export default {
    components: {History, ItemErrors, ItemShowActions},
    data () {
      return {
        historyKey: 1,
      }
    },
    computed: mapGetters({
      item: 'contact/item'
    }),
    created() {
      this.getItem(this.$route.params.id)
    },
    beforeDestroy() {
      this.reset()
    },
    methods: {
      ...mapActions({
        getItem: 'contact/getItem',
        reset: 'contact/reset'
      }),
    }
  }
</script>
