<template>
  <form @submit.prevent="handleSubmit(item)">
    <section class="content">
      <item-errors :entity="'template'"></item-errors>

      <div class="box box-primary">
        <div class="box-body">
          <form-input :item="item" :errors="errors" :property="'name'" :label="'template.name'" @fieldUpdated="updateValue"></form-input>
          <form-select :item="item" :errors="errors" :property="'type'" :option-property="'template_type'" :label="'template.type.name'" @fieldUpdated="updateValue"></form-select>

          <div class="form-group">
            <label for="file" class="form-control-label">{{$t('template.files')}}</label>

            <div id="app">
              <vue-dropzone id="file" :options="dropOptions()" @vdropzone-success="uploaded" @vdropzone-removed-file="removed"></vue-dropzone>
            </div>
          </div>
        </div>
      </div>

      <item-edit-actions :item="item" :entity="'Template'" :path="'template'"></item-edit-actions>
    </section>
  </form>
</template>

<script>
  import { mapActions, mapGetters } from 'vuex'
  import vueDropzone from "vue2-dropzone";
  import ItemEditActions from '../../components/layout/ItemEditActions'
  import FormInput from "../../components/layout/form/FormInput";
  import FormSelect from "../../components/layout/form/FormSelect";
  import ItemErrors from "../../components/layout/errors/ItemErrors";
  import axios from '../../interceptor'

  export default {
    components: {
      ItemErrors,
      FormSelect,
      FormInput,
      vueDropzone, ItemEditActions
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
    beforeDestroy () {
      this.reset()
    },
    computed: {
      ...mapGetters({
        errors: 'template/errors'
      }),
      authHeader() {
        return 'Bearer ' + this.$store.state.auth.token
      },
    },
    methods: {
      ...mapActions({
        reset: 'template/reset'
      }),
      updateValue(property, value) {
        this.$store.commit('template/TEMPLATE_UPDATE_ITEM', {[property]: value})
      },
      dropOptions() {
        let _this = this
        return {
          url: process.env.API_URL + '/files',
          paramName: 'file', // MB
          maxFilesize: 20, // MB
          maxFiles: 20,
          createImageThumbnails: false,
          // thumbnailWidth: 200, // px
          // thumbnailHeight: 200,
          addRemoveLinks: true,
          headers: {
            'Authorization': this.authHeader
          },
          init: function () {
            let thisDropzone = this;

            if (_this.$route.params.id) {
              axios.get(process.env.API_URL + '/templates/' + _this.$route.params.id + '/files')
                .then(response => response.data)
                .then((data) => {

                  data['hydra:member'].forEach((file) => {
                    file.name = file.originalName

                    thisDropzone.emit("addedfile", file);
                    // thisDropzone.options.thumbnail.call(thisDropzone, file, process.env.API_URL + '/file/' + file.contentUrl);

                    // Make sure that there is no progress bar, etc...
                    thisDropzone.emit("complete", file);
                  })
                })
                .catch((e) => {
                  console.log('Error uploading', e)
                })
            }
          }
        }
      },
      uploaded(data) {
        this.item.files = this.item.files || []
        this.item.files.push('/files/' + JSON.parse(data.xhr.response).id)
      },
      removed(data) {
        this.item.files = this.item.files || []
        this.item.files = this.item.files.filter(function (el) {
          return el.id !== data.id;
        });
      }
    }
  }
</script>
