<template>
<form @submit.prevent="handleSubmit(item)">
  <section class="content">
    <item-errors :entity="'group'"></item-errors>

    <div class="box box-primary">
      <div class="box-body">
        <form-input :item="item" :errors="errors" :property="'name'" :label="'group.name'" @fieldUpdated="updateValue"></form-input>

        <div class="form-group" v-for="module in modules">
          <h2>{{$t('module.' + module.name.replace(/\s+/g, '_').toLowerCase())}}</h2>

          <template v-for="role in module.roles" style="margin-left: 20px;">
            <form-checkbox :item="item" :errors="errors" :property="'roles'" :label="'role.' + role.name.toLowerCase()" @fieldUpdated="updateValue"></form-checkbox>
          </template>
        </div>
      </div>
    </div>

    <item-edit-actions :item="item" :entity="'Group'" :path="'group'"></item-edit-actions>
  </section>
</form>
</template>

<script>
  import {mapActions, mapGetters} from 'vuex'
  import ItemEditActions from '../../components/layout/ItemEditActions'
  import VuePassword from 'vue-password'
  import FormInput from "../../components/layout/form/FormInput";
  import FormCheckbox from "../../components/layout/form/FormCheckbox";
  import ItemErrors from "../../components/layout/errors/ItemErrors";

  export default {
    components: {ItemErrors, FormCheckbox, FormInput, VuePassword, ItemEditActions},
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
        errors: 'group/errors'
      }),
      modules() {
        return this.$store.getters['module/items'] || []
      },
    },
    created() {
      this.getModules('/modules?pagination[itemsPerPage]=500')
    },
    methods: {
      ...mapActions({
        reset: 'group/reset',
        getModules: 'module/getItems'
      }),
      updateValue(property, value) {
        this.$store.commit('group/GROUP_UPDATE_ITEM', {[property]: value})
      }
    }
  }
</script>
