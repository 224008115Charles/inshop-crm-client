<template>
  <form @submit.prevent="handleSubmit">
    <section class="content">
      <item-errors :entity="'category'"></item-errors>

      <div class="nav-tabs-custom">
        <ul class="nav nav-tabs">
          <li :class="{active: i === 0}" v-for="(language, i) in languages" :key="'header_' + language.id">
            <a :href="'#' + language.code" data-toggle="tab" aria-expanded="false">{{language.name}}</a>
          </li>
        </ul>
        <div class="tab-content">
          <div :class="['tab-pane', {active: i === 0}]" :id="language.code" v-for="(language, i) in languages" :key="'content_' + language.id">
            <form-input :item="findItem(language)" :errors="errors" :property="'name'" :label="'category.name'" @fieldUpdated="(property, value) => {updateTranslatedValue(property, value, language)}"></form-input>
            <form-textarea :item="findItem(language)" :errors="errors" :property="'description'" :label="'category.description'" @fieldUpdated="(property, value) => {updateTranslatedValue(property, value, language)}"></form-textarea>
          </div>
        </div>
      </div>

      <div class="box box-primary">
        <div class="box-body">
          <form-select :item="item" :errors="errors" :property="'parent'" :option-property="'category'" :label="'category.parent.name'" @fieldUpdated="updateValue"></form-select>
          <form-number :item="item" :errors="errors" :property="'position'" :label="'category.position'" @fieldUpdated="updateValue"></form-number>
          <form-checkbox :item="item" :errors="errors" :property="'isActive'" :label="'category.isActive'" @fieldUpdated="updateValue"></form-checkbox>
        </div>
      </div>

      <item-edit-actions :item="item" :entity="'Category'" :path="'category'"></item-edit-actions>
    </section>
  </form>
</template>

<script>
  import ItemEditActions from '../../components/layout/ItemEditActions'
  import {mapActions, mapGetters} from 'vuex'
  import FormSelect from "../../components/layout/form/FormSelect";
  import FormInput from "../../components/layout/form/FormInput";
  import FormCheckbox from "../../components/layout/form/FormCheckbox";
  import ItemErrors from "../../components/layout/errors/ItemErrors";
  import FormNumber from "../../components/layout/form/FormNumber";
  import FormTextarea from "../../components/layout/form/FormTextarea";

  export default {
    components: {
      FormTextarea,
      FormNumber,
      ItemErrors,
      FormCheckbox,
      FormInput,
      FormSelect,
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
        errors: 'category/errors',
      }),
      languages() {
        return this.$store.getters['language/items'] || []
      },
      translations: {
        get() {
          let translations = [];

          this.languages.forEach(language => {
            translations.push(this.findItem(language))
          });

          return translations
        },
        set(v) {
          console.log('set', v)
        }
      },
    },
    created() {
      this.getLanguages()
    },
    methods: {
      ...mapActions({
        reset: 'category/reset'
      }),
      findItem(language) {
        let translation = {
          language: language,
        };

        this.item.translations.some(element => {
          if (element.language.code === language.code) {
            translation = element
          }
        });

        return translation
      },
      updateValue(property, value) {
        this.$store.commit('category/CATEGORY_UPDATE_ITEM', {[property]: value})
      },
      updateTranslatedValue(property, value, language) {
        this.$store.commit('category/CATEGORY_UPDATE_ITEM_TRANSLATION', {
          property: property,
          value: value,
          language: language,
        })
      },
      ...mapActions({
        getLanguages: 'language/getItems',
      }),
    }
  }
</script>
