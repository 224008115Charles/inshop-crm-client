<template>
  <form @submit.prevent="handleSubmit(item)">
    <section class="content">
      <item-errors :entity="'vat'"></item-errors>

      <div class="box box-primary">
        <div class="box-body">
          <form-input :item="item" :errors="errors" :property="'name'" :label="'vat.name'" @fieldUpdated="updateValue"></form-input>
          <form-number :item="item" :errors="errors" :property="'value'" :label="'vat.value'" @fieldUpdated="updateValue"></form-number>
        </div>
      </div>

      <item-edit-actions :item="item" :entity="'Vat'" :path="'vat'"></item-edit-actions>
    </section>
  </form>
</template>

<script>
  import {mapActions, mapGetters} from 'vuex'
  import ItemEditActions from '../../components/layout/ItemEditActions'
  import FormInput from "../../components/layout/form/FormInput";
  import FormNumber from "../../components/layout/form/FormNumber";
  import ItemErrors from "../../components/layout/errors/ItemErrors";

  export default {
    components: {
      ItemErrors,
      FormNumber,
      FormInput,
      ItemEditActions
    },
    props: {
      handleSubmit: {
        type: Function,
        required: true
      },
      item: {
        type: Object,
        required: true
      }
    },
    beforeDestroy() {
      this.reset()
    },
    computed: {
      ...mapGetters({
        errors: 'vat/errors'
      })
    },
    methods: {
      ...mapActions({
        reset: 'vat/reset'
      }),
      updateValue(property, value) {
        this.$store.commit('vat/VAT_UPDATE_ITEM', {[property]: value})
      }
    }
  }
</script>
