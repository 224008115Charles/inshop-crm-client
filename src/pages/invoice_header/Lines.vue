<template>
  <div class="table-responsive">
    <table class="table table-striped table-hover">
      <thead>
      <tr>
        <th style="width: 75px;">{{$t('invoice_line.id')}}</th>
        <th>{{$t('invoice_line.product.name')}}</th>
        <th>{{$t('invoice_line.name')}}</th>
        <th>{{$t('invoice_line.vat.name')}}</th>
        <th>{{$t('invoice_line.unitPriceNetto')}}</th>
        <th>{{$t('invoice_line.unitsCount')}}</th>
        <th>{{$t('invoice_line.createdAt')}}</th>
        <th>{{$t('invoice_line.updatedAt')}}</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="line in lines" :key="line.id">
        <td>
          {{line.id}}
        </td>
        <td>
          <router-link v-if="line['product']" :to="{name: 'ProductShow', params: { id: line['product'].id }}">
            {{ line['product']['name'] }}
          </router-link>
        </td>

        <td>
          {{line.name}}
        </td>
        <td>
          <router-link v-if="line['vat']" :to="{name: 'VatShow', params: { id: line['vat'].id }}">
            {{ line['vat']['name'] }}
          </router-link>
        </td>

        <td>
          {{line.unitPriceNetto}}
        </td>

        <td>
          {{line.unitsCount}}
        </td>
        <td>
          {{ moment(line['createdAt']).format('DD-MM-YYYY HH:mm') }}
        </td>
        <td>
          {{ moment(line['updatedAt']).format('DD-MM-YYYY HH:mm') }}
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'Lines',
    props: {
      lines: {
        type: Array,
        default: function () {
          return []
        }
      }
    }
  }
</script>
