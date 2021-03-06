<template>
  <div>
    <section class="content-header">
      <h1>{{ item && item['number'] }}</h1>
    </section>

    <section class="content">
      <item-errors :entity="'invoice_header'"></item-errors>

      <div class="nav-tabs-custom">
        <ul class="nav nav-tabs">
          <li class="active"><a href="#general" data-toggle="tab" aria-expanded="false">{{$t('invoice_header.tabs.general')}}</a></li>
          <li><a href="#lines" data-toggle="tab" aria-expanded="false">{{$t('invoice_header.tabs.lines')}}</a></li>
          <li><a href="#history" data-toggle="tab" aria-expanded="false">{{$t('tabs.history')}}</a></li>
        </ul>
        <div class="tab-content">
          <div class="tab-pane active" id="general">
            <div v-if="item" class="table-responsive">
              <table class="table table-striped table-hover">
                <thead>
                <tr>
                  <th width="20%">{{$t('field')}}</th>
                  <th>{{$t('value')}}</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                  <td>{{$t('invoice_header.number')}}</td>
                  <td>{{ item['number'] }}</td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.status.name')}}</td>
                  <td>
                    <router-link v-if="item['status']" :to="{name: 'InvoiceStatusShow', params: { id: item['status'].id }}">
                      {{ item['status']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.type.name')}}</td>
                  <td>
                    <router-link v-if="item['type']" :to="{name: 'InvoiceTypeShow', params: { id: item['type'].id }}">
                      {{ item['type']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.orderHeader.number')}}</td>
                  <td>
                    <router-link v-if="item['orderHeader']" :to="{name: 'OrderHeaderShow', params: { id: item['orderHeader'].id }}">
                      {{ item['orderHeader']['number'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.companyFrom.name')}}</td>
                  <td>
                    <router-link v-if="item['companyFrom']" :to="{name: 'CompanyShow', params: { id: item['companyFrom'].id }}">
                      {{ item['companyFrom']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.companyTo.name')}}</td>
                  <td>
                    <router-link v-if="item['companyTo']" :to="{name: 'CompanyShow', params: { id: item['companyTo'].id }}">
                      {{ item['companyTo']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.agreement.name')}}</td>
                  <td>
                    <router-link v-if="item['agreement']" :to="{name: 'DocumentShow', params: { id: item['agreement'].id }}">
                      {{ item['agreement']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.currency.name')}}</td>
                  <td>
                    <router-link v-if="item['currency']" :to="{name: 'CurrencyShow', params: { id: item['currency'].id }}">
                      {{ item['currency']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.language.name')}}</td>
                  <td>
                    <router-link v-if="item['language']" :to="{name: 'LanguageShow', params: { id: item['language'].id }}">
                      {{ item['language']['name'] }}
                    </router-link>
                  </td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.dateOfInvoice')}}</td>
                  <td>{{ moment(item['dateOfInvoice']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.dateOfSale')}}</td>
                  <td>{{ moment(item['dateOfSale']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.maturity')}}</td>
                  <td>{{ item['maturity'] }}</td>
                </tr>

                <tr>
                  <td>{{$t('invoice_header.createdAt')}}</td>
                  <td>{{ moment(item['createdAt']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>
                <tr>
                  <td>{{$t('invoice_header.updatedAt')}}</td>
                  <td>{{ moment(item['updatedAt']).format('DD-MM-YYYY HH:mm') }}</td>
                </tr>
                <tr>
                  <td>{{$t('invoice_header.updatedBy')}}</td>
                  <td>{{ item['updatedBy'] }}</td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>

          <div class="tab-pane" id="lines">
            <div v-if="item.lines" class="table-responsive">
              <lines :lines="item.lines"></lines>
            </div>
          </div>
          <div class="tab-pane" id="history">
            <history :id="parseInt($route.params.id)" :entity="'InvoiceHeader'" :path="'invoice_header'" :key="historyKey"></history>
          </div>
        </div>
      </div>

      <item-show-actions :item="item" :entity="'InvoiceHeader'" :path="'invoice_header'"></item-show-actions>
    </section>
  </div>
</template>

<script>
  import {mapActions, mapGetters} from 'vuex'
  import ItemShowActions from '../../components/layout/ItemShowActions'
  import ItemErrors from '../../components/layout/errors/ItemErrors'
  import Lines from './Lines'
  import History from '../../components/History'

  export default {
    components: {History, Lines, ItemErrors, ItemShowActions},
    data () {
      return {
        historyKey: 1,
      }
    },
    computed: mapGetters({
      item: 'invoice_header/item',
    }),
    created() {
      this.getItem(this.$route.params.id)
    },
    beforeDestroy() {
      this.reset()
    },
    methods: {
      ...mapActions({
        getItem: 'invoice_header/getItem',
        reset: 'invoice_header/reset',
      })
    }
  }
</script>
